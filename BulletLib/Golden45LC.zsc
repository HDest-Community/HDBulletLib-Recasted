class HDB_Gold45lc:HDBulletActor{
	default{
		pushfactor 0.5;//big and slow, not as accurate
		mass 375;//gold is fucking heavy
		speed HDCONST_MPSTODUPT*475;//i am speed
		accuracy 100;//flat nose tip
		stamina 1150;//11.5 bullet diameter
		woundhealth 25;//more fragmentation than
		hdbulletactor.hardness 1;//gold-core rounds
	}
  
	//when a bullet hits a flat or wall
	//add 999 to "hitpart" to use the tier # instead
	override void HitGeometry(
		line hitline,
		sector hitsector,
		int hitside,
		int hitpart,
		vector3 vu,
		double lastdist
	){
		double pen=penetration();
		//TODO: MATERIALS AFFECTING PENETRATION AMOUNT
		//(take these fancy todos with a grain of salt - we may be reaching computational limits)

		setorigin(pos-vu,false);
		if(pen>1)A_SprayDecal(GetBulletDecal(speed,hitline,hitpart,false),4);
		setorigin(pos+vu,false);

		//inflict damage on destructibles
		//GZDoom native first
		int geodmg=int(pen*(1+pushfactor));
		if(hitline){
			destructible.DamageLinedef(hitline,self,geodmg,"piercing",hitpart,pos,false);
		}
		if(hitsector){
			switch(hitpart-999){
			case TIER_Upper:
				hitpart=SECPART_Ceiling;
				break;
			case TIER_Lower:
				hitpart=SECPART_Floor;
				break;
			case TIER_FFloor:
				hitpart=SECPART_3D;
				break;
			default:
				if(hitpart>=999)hitpart=SECPART_Floor;
				break;
			}
			destructible.DamageSector(hitsector,self,geodmg,"piercing",hitpart,pos,false);
		}

		//then doorbuster
		doordestroyer.destroydoor(self,10*pen*0.001*stamina,frandom(stamina*0.0006,pen*0.00005*stamina),1);


		puff();

		//in case the puff() detonated or destroyed the bullet
		if(!self||!bmissile)return;

		//everything below this should be ricochet or penetration
		if(pen<1.){
			bulletdie();
			return;
		}

		//see if the bullet ricochets
		bool didricochet=false;
		//TODO: don't ricochet on meat, require much shallower angle for liquids

		//if impact is too steep, randomly fail to ricochet
		double maxricangle=frandom(90,120)-pen-hardness;

		if(hitline){
			//angle of line
			//above plus 180, normalized
			//pick the one closer to the bullet's own angle

			//deflect along the line
			if(lastdist>128){ //to avoid infinite back-and-forth at certain angles
				double aaa1=hdmath.angleto(hitline.v1.p,hitline.v2.p);
				double aaa2=aaa1+180;
				double ppp=angle;

				double abs1=absangle(aaa1,ppp);
				double abs2=absangle(aaa2,ppp);
				double hitangle=min(abs1,abs2);

				if(hitangle<maxricangle){
					didricochet=true;
					double aaa=(abs1>abs2)?aaa2:aaa1;
					vel.xy=rotatevector(vel.xy,deltaangle(ppp,aaa)*frandom(1.,1.05));

					//transfer some of the deflection upwards or downwards
					double vlz=vel.z;
					if(vlz){
						double xyl=vel.xy.length()*frandom(0.9,1.1);
						double xyvlz=xyl+vlz;
						vel.z*=xyvlz/xyl;
						vel.xy*=xyl/xyvlz;
					}
					vel.z+=frandom(-0.01,0.01)*speed;
					vel*=1.-hitangle*0.011;
				}
			}
		}else if(
			hitpart==SECPART_Floor
			||hitpart==SECPART_Ceiling
		){
			bool isceiling=hitpart==SECPART_CEILING;
			double planepitch=0;

			//get the relative pitch of the surface
			if(lastdist>128){ //to avoid infinite back-and-forth at certain angles
				double zdif;
				if(checkmove(pos.xy+vel.xy.unit()*0.5))zdif=getzat(0.5,flags:isceiling?GZF_CEILING:0)-pos.z;
				else zdif=pos.z-getzat(-0.5,flags:isceiling?GZF_CEILING:0);
				if(zdif)planepitch=atan2(zdif,0.5);

				planepitch+=frandom(0.,1.);
				if(isceiling)planepitch*=-1;

				double hitangle=absangle(-pitch,planepitch);
				if(hitangle>90)hitangle=180-hitangle;

				if(hitangle<maxricangle){
					didricochet=true;
					//at certain angles the ricochet should reverse xy direction
					if(hitangle>90){
						//bullet ricochets "backward"
						pitch=planepitch;
						angle+=180;
					}else{
						//bullet ricochets "forward"
						pitch=-planepitch;
					}
					speed*=(1-frandom(0.,0.02)*(7-hardness)-(hitangle*0.003));
					A_ChangeVelocity(cos(pitch)*speed,0,sin(-pitch)*speed,CVF_RELATIVE|CVF_REPLACE);
					vel*=1.-hitangle*0.011;
				}
			}
		}

		//see if the bullet penetrates
		if(!didricochet){
			//calculate the penetration distance
			//if that point is in the map:
			vector3 pendest=pos;
			bool dopenetrate=false; //"dope netrate". sounds pleasantly fast.
			int penunits=0;
			for(int i=0;i<pen;i++){
				pendest+=vu;
				if(
					level.ispointinlevel(pendest)
					//performance???
					//&&pendest.z>getzat(pendest.x,pendest.y,0,GZF_ABSOLUTEPOS)
					//&&pendest.z<getzat(pendest.x,pendest.y,0,GZF_CEILING|GZF_ABSOLUTEPOS)
				){
					dopenetrate=true;
					penunits=i;
					break;
				}
			}
			if(dopenetrate){
				//warp forwards to that distance
				setorigin(pendest,true);
				realpos=pendest;

				//do a REGULAR ACTOR linetrace
				angle-=180;pitch=-pitch;
				flinetracedata penlt;
				LineTrace(
					angle,
					pen+1,
					pitch,
					flags:TRF_THRUACTORS|TRF_ABSOFFSET,
					data:penlt
				);

				//move to emergence point and spray a decal
				setorigin(pendest+vu*0.3,true);
				puff();
				A_SprayDecal(GetBulletDecal(speed,hitline,hitpart,true));
				angle+=180;pitch=-pitch;

				if(penlt.hittype==TRACE_HitActor){
					//if it hits an actor, affect that actor
					onhitactor(penlt.hitactor,penlt.hitlocation,vu);
					if(penlt.hitactor)traceactors.push(penlt.hitactor);
				}

				//reduce momentum, increase tumbling, etc.
				angle+=frandom(-pushfactor,pushfactor)*penunits;
				pitch+=frandom(-pushfactor,pushfactor)*penunits;
				speed=max(0,speed-frandom(-pushfactor,pushfactor)*penunits*10);
				A_ChangeVelocity(cos(pitch)*speed,0,-sin(pitch)*speed,CVF_RELATIVE|CVF_REPLACE);
			}else{
				puff();
				bulletdie();
				return;
			}
		}

		//update realpos to keep these values in sync
		realpos=pos;

		//warp the bullet
		hardness=max(1,hardness-random(0,random(0,1)));
		stamina=max(1,stamina+random(0,(stamina>>1)));
	}

}

class HDSpentGold45lc:HDDebris{
	default{
		bouncesound "misc/casing";scale 0.3;
	}
	states{
	spawn:
		C45G A 2 nodelay{
			A_SetRoll(roll+45,SPF_INTERPOLATE);
		}loop;
	death:
		C45G B -1;
	}
}

class HDGold45LCAmmo:HDRoundAmmo{
	default{
		+inventory.ignoreskill
		+cannotpush
		+forcexybillboard
		+rollsprite +rollcenter
		+hdpickup.multipickup
		xscale 0.4;
		yscale 0.4;
		inventory.pickupmessage "Picked up a gold 45. LC round.";
		hdpickup.refid "45g";
		tag "gold 45. lc round";
		hdpickup.bulk ENC_9*3;
		inventory.icon "GLBXA0";
	}
	override void SplitPickup(){
		SplitPickupBoxableRound(6,12,"HDGold45lcBoxPickup","GSIXA0","GC45A0");
	}
	override void GetItemsThatUseThis(){
		itemsthatusethis.push("HDGoldSingleActionRevolver");
	}
	states{
	spawn:
		GC45 A -1;
		GSIX A -1;
	}
}

class HDGold45lcBoxPickup:HDUPK{
	default{
		//$Category "Ammo/Hideous Destructor/"
		//$Title "Box of gold .45"
		//$Sprite "GLBXA0"
		scale 0.4;
		hdupk.amount 12;
		hdupk.pickupsound "weapons/pocket";
		hdupk.pickupmessage "Picked up some gold .45LC ammo.";
		hdupk.pickuptype "HDGold45LCAmmo";
	}
	states{
	spawn:
		GLBX A -1;
	}
}

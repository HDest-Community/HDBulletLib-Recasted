class HDB_45lc:HDBulletActor{
	default{
		pushfactor 0.5;//big and slow, not as accurate
		mass 225; // over twice the mass of 355
		speed HDCONST_MPSTODUPT*275;//much slower than 355
		accuracy 100;//flat nose tip
		stamina 1150;//11.5 bullet diameter
		woundhealth 25;//more fragmentation than
		hdbulletactor.hardness 2;//Jacketed Soft Points
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

class HDSpent45LC:HDSpent9mm{default{xscale 0.75; yscale 1.0;}}

class HD45LCAmmo:HDPistolAmmo{
	default{
		xscale 0.8;
		yscale 1.0;
		inventory.pickupmessage "Picked up a .45LC round.";
		hdpickup.refid "45L";
		tag ".45LC round";
		hdpickup.bulk ENC_355*2;
  inventory.icon "T10MA0";
	}

	override void GetItemsThatUseThis(){
		itemsthatusethis.push("HDSingleActionRevolver");
	}

	override void SplitPickup(){
	  SplitPickupBoxableRound(10, 50, "HD45LCBoxPickup",  "T10MA0", "PR10A0");
	}

}
class HD45LCBoxPickup:HDUPK{
	default{
		//$Category "Ammo/Hideous Destructor/"
		//$Title "Box of .45"
		//$Sprite "45LBA0"
		scale 0.4;
		hdupk.amount 50;
		hdupk.pickupsound "weapons/pocket";
		hdupk.pickupmessage "Picked up some .45LC ammo.";
		hdupk.pickuptype "HD45LCAmmo";
	}
	states{
	spawn:
		45LB A -1;
	}
}

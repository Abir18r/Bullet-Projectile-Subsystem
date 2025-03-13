# Advanced Bullet Projectile System

## Overview
This system provides a robust, high-performance bullet projectile simulation for Unreal Engine. It uses the **Gameplay Ability System (GAS)** for hit notifications and features advanced physics calculations, including penetration, ricochet, and drag force. 

## Features
- **Bullet Handling:** Manages bullet movement, hit detection, and physics interactions.
- **Penetration & Ricochet:** Determines whether a bullet penetrates or bounces based on velocity and material properties.
- **Wind Effects:** Adjusts trajectory based on nearby wind sources.
- **Debugging Tools:** Visualizes projectile path and impacts for development.
- **Subsystem Architecture:** Centralized handling of projectile instances.

## Components

### 1. `FBulletProjectileHandle` (BulletProjectileHandle.h & .cpp)
Handles individual bullet movement, collision detection, and interaction logic.
- **Functions:**
  - `ProjectileInitialize(UWorld* World)`: Initializes projectile velocity and effects.
  - `PerformBulletProjectile(float DeltaTime, const FVector& Wind)`: Moves bullet and checks for impacts.
  - `HandlePenetration() & HandleRicochet()`: Processes physics-based interactions.
  - `ComputeGravityAndDeceleration()`: Adjusts bullet velocity due to air resistance and gravity.

### 2. `UBulletProjectileSubsystem` (BulletProjectileSubsystem.h & .cpp)
Manages all active bullets, updating their movement each tick.
- **Functions:**
  - `FireProjectile()`: Spawns a new bullet instance.
  - `PerformProjectileStep()`: Updates individual bullet states.
  - `GetWindSourceVelocity()`: Retrieves wind velocity affecting bullet trajectory.

### 3. `UProjectileData` (ProjectileData.h & .cpp)
Stores bullet properties such as speed, mass, and lifespan.
- **Key Properties:**
  - `ProjectileParams`: Defines bullet speed, mass, and lifespan.
  - `ParticleData`: Stores visual effects for projectiles.
  - `DebugData`: Enables debugging visualization.

### 4. `UPhysMaterialDataAsset` (PhysMaterialDataAsset.h & .cpp)
Defines material penetration properties.
- **Key Properties:**
  - `SurfacePropertiesMap`: Maps physical surfaces to penetration data.
  - `MinVelocityToPenetrate`: Threshold velocity required for penetration.

## Usage
1. **Setup a projectile data asset (`UProjectileData`)** with appropriate physics properties.
2. **Call `FireProjectile()`** from `UBulletProjectileSubsystem` to launch a bullet.
3. **Customize materials (`UPhysMaterialDataAsset`)** to control penetration behavior.
4. **Enable debug options** to visualize projectile behavior during development.

# How to Use
## C++:

1. **Include the subsystem header:**
   ```cpp
   #include "BulletProjectileSubsystem.h"
   ```
2. **Obtain a reference to the subsystem:**
   ```cpp
   UBulletProjectileSubsystem* ProjectileSubsystem = GetWorld()->GetSubsystem<UBulletProjectileSubsystem>();
   ```
3. **Define launch parameters and fire the projectile:**
   ```cpp
   FVector StartLocation = Instigator->GetActorLocation();
   FTransform LaunchTransform(FRotator::ZeroRotator, StartLocation);
   ProjectileSubsystem->FireProjectile(Instigator, 1, BulletData, {}, LaunchTransform, OnBulletProjectileHit, OnBulletHitImpact, OnBulletHitRicochet);
   ```
## Blueprint Implementation:

![Bullet Projectile Blueprint Implementation](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/BPS_Implementation_001.png)
### Add Projectile data by projectile data asset
![Add Bullet Projectile Data Asset](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/BPS_ProjectileData.png)

### Add Physical surface in Projects Settings
![Physical Surface](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/Physical%20Surface.png)

![Surface Data](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/SurfaceData.png)

![Joules](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_Joules.png)

![Impact Angle Gradient](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_ImpactAngle.png)

![Impact Angle Z Intercept](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_ImpactZIntercept.png)

![Velocity Gradient](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_VelocityGradient.png)

![Velocity Z Intercept](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_VelocityZ.png)

![Min Velocity to Penetrate](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSM_MiniVelocity.png)

### Add Surface data to projectile data asset
![DA_Surface to DA_Projectile](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/PSDA_SurfaceData.png)

### Select collision channel according to your needs
![Collision Channel](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/Collision%20Channel.png)

### GameplayCue Notify tag for GCN Impact
![GCN Impact Tag](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/ImpactTag.png)

### GameplayCue Notify tag for GCN Ricochet
![GCN Ricochet Tag](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/RicochetTag.png)

### Create individual GameplayNotifyCue_Burst (GCN) for Impact and ricochet
![GCN(Burst)](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/GCN_BlueprintCreate.png)

### Add specific tag to the GCN Impact & Ricocheet
![GCN tag](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/GCN_Burst_Rifle.png)

### Customize Burst Effects like decals, sounds, particles effects
![GCN_Effects](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/GCN_Rifle_Effects.png)

## Debugging
- **Enable `bDebugPath`** in `FProjectileDebugData` to visualize bullet trajectory.
- **Adjust `ImpactRadius`** to highlight impact points.
- **Use `DrawDebugLine()`** calls for real-time debugging.
![DA_Projectile Debug](https://github.com/Abir18r/Bullet-Projectile-Subsystem/blob/main/Images/Debug.png)

## Dependencies
- Unreal Engine GAS (Gameplay Ability System)
- Niagara for particle effects
- WindDirectionalSourceComponent for wind simulation

## Future Enhancements
- Improved bullet spread calculations
- Advanced material-based deformation effects
- Optimized memory management for high bullet counts

## Support

For support, Join Discord Server https://discord.gg/7nZ5UjE6

---
**Author:** Md Rahat Chowdhury


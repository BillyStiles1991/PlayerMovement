# PlayerMovement

# Unity Scripts Collection and Unity inspector information - Player Movement + Camera Follow (2D)

## 1) Overview
This Unity project is a simple 2D character controller setup with:
- **Responsive left/right movement** using acceleration.
- **Jumping** when grounded.
- **A melee attack** that enables a hitbox briefly.
- **A camera script** that tracks the player’s position.

It’s intended as a foundation for a 2D platformer (player controller + basic combat + camera follow).

---

## 2) Script descriptions (what each script does)

### MainCharacterController.cs
Controls the player character’s core gameplay actions:

**Movement**
- Reads horizontal input (`Input.GetAxisRaw("Horizontal")`).
- Moves the Rigidbody2D toward a target speed using acceleration (`Mathf.MoveTowards`).
- Flips the character sprite when switching direction.

**Ground checking + jump**
- Checks if the player is grounded using `Physics2D.OverlapCircle` at a `groundCheck` Transform.
- Allows jump only when grounded (`Input.GetButtonDown("Jump")`).

**Attack**
- Triggers an attack when pressing:
  - **E (keyboard)** or
  - **Fire1 (controller / default input mapping)**
- Enables a referenced `attackHitbox` GameObject for `0.3` seconds, then disables it.

**Damage + knockback**
- On trigger contact with objects tagged `"Enemy"`, it applies knockback away from the enemy.
- Starts an invincibility coroutine (`invincibilityDuration`), though note:
  - The `isInvincible` flag is set but currently **not used** to block repeated hits. (You can add a check so the player can’t be knocked back repeatedly during invincibility.)
-**Note:** For the purpose of this project this part of the script is not used. Will be implimented at a later date when enemies are added.

---

### FollowCam.cs
A simple camera-follow script:

- Keeps the camera’s **x and y** aligned to a target Transform each frame.
- Does not add smoothing or clamping (camera will “snap” to the target).
- Works best with a target point ahead and above of the Main player 

---

## 3) How to set up the project in the Unity Inspector

### A) Scene + layers/tags setup
1. **Create a Ground layer**
   - Go to **Edit → Project Settings → Tags and Layers**
   - Add a layer called: `Ground`

---

### B) Player GameObject setup (MainCharacterController)
1. **Create / select your Player GameObject**
2. Add required components:
   - **Rigidbody2D**
     - Recommended: `Gravity Scale` > 0 
     - `Body Type`: Dynamic
     - Enable `Freeze Rotation Z` to prevent tipping.
   - **Collider2D** (BoxCollider2D or CapsuleCollider2D)
   - **SpriteRenderer**
3. Add the script:
   - Attach **`MainCharacterController.cs`** to the Player object.

#### Inspector fields to assign on `MainCharacterController`
- **Spawn Point (Transform)** *(optional in current script)*
  - Create an empty GameObject called `SpawnPoint` and drag it in.
  - Note: currently not used in code, but will be required after future code updates.
- **Ground Check (Transform)** *(required)*
  1. Create an empty child object on Player called `GroundCheck`
  2. Position it at the player’s feet (at the bottom of the collider).
  3. Drag it into the script field.
- **CameraTracker (Transform)** *(required)*
  1. Create an empty child object on Player called `CameraTracker`
  2. Position it at ahead of the player and slightly above(adjust to your desired tracking location).
- **Ground Layer (LayerMask)** *(required)*
  - Select the `Ground` layer.
- **Ground Check Radius (float)**
  - Default is `0.2`; adjust if needed.
- **Attack Hitbox (GameObject)** *(required for attacking)*
  1. Create a child object called `AttackHitbox`
  2. Add a **Collider2D** (often BoxCollider2D)
     - Set collider `Is Trigger` = **true**
  3. Position it in front of the player (where the attack should land)
  4. **Disable it in the Inspector by default** (unchecked)
  5. Drag this object into `attackHitbox` on the Player script.

---

### D) Camera setup (FollowCam)
1. Select your **Main Camera**
2. Attach **`FollowCam.cs`**
3. Assign the **Target** field:
   - Drag your Player’s **Camera Tracker** transform into the `target` slot.

---

## Files included
- `MainCharacterController.cs` — player movement, jump, attack hitbox toggle, enemy knockback
- `FollowCam.cs` — camera follows the player transform on x/y

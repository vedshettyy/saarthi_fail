# Saarthi Village VR

A walkable Indian village, Shivpur, built around Sahakar Bank. The Saarthi welfare scheme kiosk stands at the bank's front corner, right after the ATM. It runs in a desktop browser and in VR headsets such as Quest.

## What is in the village

| Area | What you find |
|---|---|
| Main road | Murram road with tyre ruts, entrance arch, milestones, electric poles with sagging wires |
| North side | Gram panchayat office with the national flag, tiled and tin roof houses, kirana shop, Sahakar Bank, ATM booth, Saarthi kiosk, medical shop, huts |
| Village square | Banyan tree on a stone katta with elders sitting, hand pump, temple with a saffron flag, vegetable and chai stalls |
| South side | Houses with verandahs, rangoli, tulsi planters, rope cots, clothes lines, round and square thatched huts |
| Outskirts | Crop fields and sugarcane, haystacks, a well, a scarecrow, a parked tractor, cows and buffaloes, rolling hills |
| On the move | Tractor with a hay trolley, bullock cart, shared auto, motorbikes, a car, villagers and children |

The kiosk flow is the same as the earlier version: answer a few questions and Saarthi suggests matching government schemes.

## Run it

The scene loads files from the `assets` folder, so it must be served over http, not opened by double clicking.

**On your computer**

```
cd saarthi-village
python3 -m http.server 8000
```

Then open http://localhost:8000 in Chrome. On Windows use `py -m http.server 8000`.

**On GitHub Pages (works on Quest and laptops)**

1. Create a public repository on github.com.
2. Click "uploading an existing file" and drag in everything inside this folder (50 files, each under 9.5 MB, so one upload is enough), so `index.html` sits at the top level next to the `assets` folder. Commit.
   If the page shows a red message about the **assets** folder, that folder did not upload: drag it in again.
3. Go to Settings, then Pages. Choose "Deploy from a branch", `main`, `/ (root)`, and Save.
4. After a minute or two, open `https://yourusername.github.io/your-repo-name/`. On Quest, press Enter VR.

## Performance

Built to stay smooth on a Quest headset while looking detailed on a laptop.

- **People**: all 49 villagers are different Microsoft Rocketbox avatars, chosen by role (elders on the katta, business wear for bank staff, a pharmacist, a security guard, a gardener, a woodcutter, a delivery rider, children). Each has a detailed version (Rocketbox's high detail head, 1024 px skin with a roughness map for natural shine) within 12 m (7 m on Quest) and a light version (about 4,000 triangles) further away. Files are compressed with WebP textures and quantized meshes, and each person's pieces are merged into 3 draw calls when loaded.
- **Seated people** are placed by their pelvis bone onto the actual seat (chairs, katta, auto, tractor, cart, motorbike, car), so nobody floats above or sticks out of a vehicle.
- **Static objects** (houses, bank, trees, stalls, vehicles) are merged by material when the page loads, which cuts draw calls several times over.
- **Grass, crops, sugarcane and distant trees** are drawn as instances: one draw call per kind.
- **Quest mode** (automatic on Quest, or add `?lite`) uses 1K shadows refreshed every third frame, no post effects, fewer grass tufts and crops, and fewer animated people.
- **Desktop** adds 4K shadows and ambient occlusion. Add `?ao=0` to turn ambient occlusion off.

## Controls

| Action | Desktop | VR |
|---|---|---|
| Walk | WASD or arrow keys, Shift to go faster | Left stick |
| Look | Mouse | Head, right stick snap turn |
| Use the kiosk | Aim the dot at the screen, click | Point and pull the trigger |
| Talk to someone | T | Point and pull the trigger |
| Hold an object | E | Grip |
| Evening lighting | N | Button on screen |
| Hide the help panel | H | none |
| Back to start | none | B or Y |

## Add your own 3D models

Drop `.glb` files into `assets/models/` with these exact names and they replace the built in versions, scaled automatically:

| File | Replaces | Fitted to |
|---|---|---|
| `cow.glb` | cows, buffaloes and the cart bullocks | 2.2 m long |
| `tractor.glb` | the parked tractor | 3.6 m long |
| `auto.glb` | the auto rickshaw | 2.7 m long |
| `car.glb` | the car | 3.9 m long |
| `scooter.glb` | motorbikes | 1.8 m long |
| `tree.glb` | neem and mango trees | 6 m tall |

If a model faces the wrong way, change its `rot` value in the `MODELS` block near the top of `index.html` (for example `Math.PI / 2`). Check the licence of any model before you publish it.

## Assets and licences

| Folder | Content | Source and licence |
|---|---|---|
| `assets/people/` | 52 avatars, each with a near and a far version, packed into `people_near_*.bin` and `people_lo_*.bin` (`index.json` lists where each one is) | Microsoft Rocketbox, MIT (see `licences/`) |
| `assets/anim/clips_*.json` | Motion capture: walk, stroll, idle, talk, wait, hold bag, sit | Microsoft Rocketbox, MIT |
| `assets/textures/` | Dirt road, grass, clay roof tiles, thatch, mud wall, laterite stone, wood, plaster, pavers, concrete, tin | Generated for this project |
| `assets/hdr/` | Daylight HDRI | Poly Haven, CC0 (via the three.js examples) |

Shivpur, Sakharwadi and Sahakar Bank are fictional names.

# Roblox-Chams-Material-POC
A POC on how to do chams through swapping Shader Programs. 

## How it works

1. The Visual Engine holds a Shader Manager, which keeps every Shader Program in a list keyed by name (`Neon`, etc.).
2. Walk that list, read each key, and take the Shader Program of the first key that contains the name you want.
3. Walk the entities in the render scene, get each entity's material, and overwrite the Shader Program in every block of the material's technique vector with the one you found.
4. The material now renders with that Shader Program, which gives the chams effect.

## Finding a Shader Program

```cpp
auto program = find_program( visual_engine, "Neon" );
```

```
Visual Engine
  +0xB98  -> Shader Manager
    +0x80 -> list head
      node:
        +0x00  next node
        +0x10  key (sso string)
        +0x30  Shader Program (shared pointer: object + ref)
```

## Finding the entities

```
Visual Engine
  +0xB40  -> scene updater
    +0x588 -> fast cluster entity manager
      array of 0x1000 nodes, 0x20 bytes each:
        +0x00  humanoid
        +0x08  fast cluster
          container +0x48 -> vector of entities
```

## Fast Cluster Entity and Material

```
Fast Cluster Entity
  +0x70  -> material

Material
  +0x00  technique vector (first, last, end)

Technique block (stride 0x88, up to 32 blocks)
  +0x28  Shader Program (shared pointer: object, ref)  <- swapped
```

## Swapping Shader Programs

To swap each shared pointer you have to copy both the object and the ref (control block)

## Demo

<img width="726" height="475" alt="image" src="https://github.com/user-attachments/assets/92511026-9ab3-489c-b760-7a37ced253a2" />
<img width="317" height="293" alt="image" src="https://github.com/user-attachments/assets/261845b9-7417-4161-a253-65adb65359e4" />

## Notes

- Offsets change between Roblox updates and have to be updated.
- The ref count is not incremented by a raw write, so restore the original Shader Programs before they are freed.
- Re-run the Shader Program search after a map or character reload, since the saved pointers can go stale.
- You can use any Shader Program not just "Neon" 

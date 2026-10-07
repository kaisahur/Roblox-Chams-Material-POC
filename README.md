# Roblox-Chams-Material-POC
A POC on how to do chams through swapping Shader Programs. 

## How it works

1. The Visual Engine holds a Shader Manager, which keeps every Shader Program in a list keyed by name.
2. Walk that list and read each key until one matches the name you want, for example `Neon`. This gives you that Shader Program.
3. Take the material of a Fast Cluster Entity and overwrite the Shader Programs in its technique vector with the one you found.
4. The material now renders with that Shader Program, which gives the chams effect.

## Demo

<img width="317" height="293" alt="image" src="https://github.com/user-attachments/assets/178dc58c-6721-4dd4-b6dd-eb159efecca4" /> <img width="726" height="475" alt="image" src="https://github.com/user-attachments/assets/33a5d598-3a41-44cf-8577-d1daf33ca1a6" />



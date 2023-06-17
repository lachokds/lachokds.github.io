---
layout: post
title: 2023-06-17-nahamcon-ctf-2023-walkthrough
date: 2023-06-17 15:07:11
categories: CTF
---

## Geosint

### Geosint 1

>[!Instructions]
>Head over to [https://osint.golf](https://osint.golf/) and select chall1 under the NahamCon2023 category. Make sure to read the directions if you are unfamiliar with Geosint/Geoguessr!
>
>Connect here: [https://osint.golf](https://osint.golf/)

![](/assets/Screenshot from 2023-06-17 16-44-46.png)

![](/assets/Screenshot from 2023-06-17 16-45-06.png)

![](/assets/Screenshot from 2023-06-17 16-45-14.png)

![](/assets/Screenshot from 2023-06-17 16-46-04.png)

![](/assets/Screenshot from 2023-06-17 16-46-18.png)

![](/assets/Screenshot from 2023-06-17 16-47-23.png)

![](/assets/Screenshot from 2023-06-17 16-48-43.png)

![](/assets/Screenshot from 2023-06-17 16-49-03.png)

![](/assets/Screenshot from 2023-06-17 16-49-28.png)

![](/assets/Screenshot from 2023-06-17 16-50-27.png)

![](/assets/Screenshot from 2023-06-17 16-50-34.png)

![](/assets/Screenshot from 2023-06-17 16-51-08.png)

![](/assets/Screenshot from 2023-06-17 16-51-39.png)

![](/assets/Screenshot from 2023-06-17 16-51-56.png)

![](/assets/Screenshot from 2023-06-17 16-52-37.png)

![](/assets/Screenshot from 2023-06-17 16-53-50.png)

![](/assets/Screenshot from 2023-06-17 16-54-55.png)

![](/assets/Screenshot from 2023-06-17 16-55-24.png)

![](/assets/Screenshot from 2023-06-17 16-56-13.png)

![](/assets/Screenshot from 2023-06-17 16-56-30.png)

> Flag `flag{3e3d01a002db29fec2a5e10ec758b852}`

### Geosint 2
>[!Instructions]
>Head over to [https://osint.golf](https://osint.golf/) and select **`chall2` under the NahamCon2023** category. Make sure to read the directions if you are unfamiliar with Geosint/Geoguessr!  
>_NOTE:_ The integrated compass direction is inconsistent with the actual panorama representation.
>
>**Connect here: [https://osint.golf](https://osint.golf/)**

- La imagen contiene un barco/ferry(?) en un muelle
- En una torre del barco se ven estas banderas

![](/assets/Pasted image 20230616211026.png)

- Luego de buscar la última con búsqueda de imágenes, encontré que es parte del [código internacional de señales](https://msi.nga.mil/api/publications/download?key=16694273/SFH00000/Pub102bk.pdf&type=view), y simboliza la letra `Z` (`Zulu`)

![](/assets/Pasted image 20230617162455.png)

- Al revisar las banderas con el código, encontré que simbolizan

```
India Bravo Quebec Zulu
```

- Es decir:

```
IBQZ
```

- Es el call sign de un barco

![](/assets/Pasted image 20230616212444.png)

- El barco está atracado en Portoferraio en Italia
- Al darle click en `Show on live map`, me tiró la ubicación dentro del muelle

![](/assets/Pasted image 20230616212722.png)

> Flag `flag{e54fcc18854f0adfdedf22120c346b3a}`

### Geosint 3
>[!Instructions]
>`chall3`

- El paisaje contiene en un lado esta vista

![](/assets/Pasted image 20230616213351.png)

- Al buscarlo en Google, pareciera ser parte del horizonte de Central Park en NY
- Específicamente, está en el área llamada Sheep Meadow

![](/assets/Pasted image 20230616214447.png)

![](/assets/Pasted image 20230616214505.png)

> Flag `flag{aa1b04ee3ded3a7c3ee16be1ff85e99c}`

## Goose Chase
>[!instructions]
>I am truly sorry. I really do apologize... I _hope_ you can bear with me as I set you all loose together, on a communal collaboriative _wild goose chase_.
>
>**Connect here:**
>[Goose Chase](https://docs.google.com/spreadsheets/d/17qy0Yw1_8rLOhrG5MWT8rWzpMi3_1vr3A_khcv3j6Cc/)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_00.03_[2023.06.17_15.09.13].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_00.15_[2023.06.17_16.38.07].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_00.27_[2023.06.17_16.38.26].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_00.50_[2023.06.17_16.38.36].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_02.39_[2023.06.17_16.38.59].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_03.01_[2023.06.17_16.39.18].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_03.22_[2023.06.17_16.39.45].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_03.25_[2023.06.17_16.39.50].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_13.21_[2023.06.17_16.41.07].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_13.32_[2023.06.17_16.41.15].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_14.31_[2023.06.17_16.41.30].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_19.43_[2023.06.17_16.42.02].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_19.54_[2023.06.17_16.42.14].jpg)

![](/assets/goose-chase-2023-06-17_11.37.42.mkv_snapshot_20.15_[2023.06.17_16.42.20].jpg)

>Flag `flag{323264294cc2a4ebb2c8d5f9e0afb0f7}`


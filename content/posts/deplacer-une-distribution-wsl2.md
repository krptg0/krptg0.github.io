+++
date = 2020-10-12T08:58:50Z
description = ""
draft = false
slug = "deplacer-une-distribution-wsl2"
title = "déplacer une distribution WSL2"
categories = ["windows"]
+++


Tout petit post d'intérêt personnel pour que je m'y retrouve. Le but est simple : déplacer une distribution WSL2 nouvellement installée vers un autre dossier.

---

```bash
# wsl --export <nom de la distro> <chemin>
wsl --export ubuntu C:\Users\linkbeat\Desktop\ubuntu.tar
# wsl --import <nom de la distro> <nouveau chemin> <chemin de l'archive>
wsl --import ubuntu2 C:\WSL\ubuntu C:\Users\linkbeat\Desktop\ubuntu.tar
```

C'est tout :)




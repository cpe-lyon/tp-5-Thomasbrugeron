### TP 5 Systèmes de fichiers, partitions et disques


### Exercice 1 : Disques et partitions

1. 

2. On affiche les disques durs avec la commande ```sudo fdisk -l``` et on remarque que le disque dur a bien été crée.

3. Pour créer les partitions de disque il faut se rendre sur le disque avec la commande ```sudo fdisk /dev/sdb```
Puis on tape n qui est la commande permettant de créer les partitions, puis on choisit s'il s'agit d'une partition principale ou non, et suite à ça on choisit la taille de la partition qu'on souhaite. 
On réalise la même action pour la deuxième partition.
Pour modifier le type de partitions on utilise la commande t puis on choisit quelle partitions on veut changer et ensuite on indique l'ID.

4. Pour formater les partitions on utilise la commande : ```sudo mkfs -t ext4 /dev/sd1/2```

5. La commande ne fonctionne pas car les partitions n'ont pas été monté. 

6. On crée tout d'abord les deux dossiers pour les points de montage data et win puis on monte les partitions dans les dossiers respectifs.
Pour les faire monter automatiquement il faut modifier le fichier fstab. 
``` /dev/sdb1 /data ext4 default 0 0 
    /dev/sdb2 /win ntfs default 0 0 
 ```

7.``` sudo mount -t ext4 /dev/sdb1 data 
 sudo mount -t ext4 /dev/sdb2 win 
    ```

### Exercice 2 : Partitionnement LVM

2.  Suite à un problème de machine virtuelle j'ai du utilisé ma deuxième pour réaliser le deuxième exercice, j'ai donc recrée un disque et ensuite crée la partition de type LVM

3.  A l'aide de la commande ```sudo pvcreate /dev/sdb1``` on crée le volume physique LVM puis on vérifie son existence avec la commande ```pvdisplay```.

4.  Par la suite le groupe de volume se crée avec la commande ```sudo vgcreate vg00 /dev/sdb1```

5. ```lvcreate -l 100%FREE -n lvData vg00```
Puis lvscan pour vérifier.

6. Comme précédemment on crée la partition puis on la formate avec la commande ```mkfs -t ext4 /dev/vg00/lvData1```
Et on modifie le fichier fstab pour la faire monter automatiquement

7. On rajoute un disque dynamique puis on recrée une partition unique LVM et un volume physique.

8. On ajoute le nouveau disque au groupe ```sudo vgextend vg00 /dev/sdc1```

9. Puis on peut modifier la taille du volume logique : ``` lvextend -L+1G /dev/vg00/lvData```
Mais tout en modifiant le file system après  ```resize2fs /dev/vmvg/Vol1```

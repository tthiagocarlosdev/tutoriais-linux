# Comandos Linux

## lsblk - *list block devices*

listar e exibir informações detalhadas sobre todos os dispositivos de bloco disponíveis no sistema

```shell
lsblk
```

```text
NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda       8:0    0  465,8G  0 disk 
├─sda1    8:1    0    512M  0 part /boot/efi
├─sda2    8:2    0    100G  0 part /
└─sda3    8:3    0    365,3G 0 part /home
sdb       8:16   1   14,9G  0 disk 
└─sdb1    8:17   1   14,9G  0 part /media/usb
```



Mostra informações detalhadas sobre os [sistemas de arquivos](https://www.certificacaolinux.com.br/comando-linux-lsblk/) e códigos UUID.

```shell
lsblk -f
```

 Inclui dispositivos vazios ou sem uso na listagem.

```shell
lsblk -a
```

Exibe apenas os discos principais, omitindo as partições individuais.

```shell
lsblk -d
```

 Permite selecionar colunas específicas para personalizar a visualização.

```shell
lsblk -o NAME,SIZE,MOUNTPOINTS
```


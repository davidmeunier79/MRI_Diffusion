# dwiprep_no_ses.sh

### Installation

Au niveau des logiciels, il faut avoir MRTRIX et FSL, et en bonus ANTS pour utiliser N4 pour la correction de biais au lieu de FSL-FAST, c’est ce que recommande mrtrix. Mais comme sur le serveur de cerimed il n’y a pas ants j’ai utilisé FSL-FAST dans mon script. Il suffit de remplacer « fsl »  par « ants »  dans les paramètres utilisateurs en début de script.

### Utilisation 

-> 1. éditez le script dans la rubrique: 

```
####### To be edited by user #####
##################################

study='Aging'
base_dir=/home/seinj/mygpdata/MRI_BIDS_DATABANK/${study}
#list_sub=$(ls -d $base_dir/sub*) # to select all subjects from the BIDS folder.
list_sub='01' #ex: '01 02 03'
acq= # add something if this flag is present in the scan name: ex: acq='_acq-1mm'
smooth=1 ## value of smoothing kernel used with fslmaths in the smoothing part at the end of the script.
bias_method='fsl' # 'fsl' or 'ants' (ants recommended but need to have ants installed)

#################################
###### end of edition ###########

```

-> 2. lancer en tapant: ``` bash dwiprep_no_ses.sh ``` dans un terminal (en étant dans le répertoire contenant la script, répertoire contenant également le fichier slspec.txt (voir plus bas) )

Le script utilise un fichier : ***slspec.txt*** (ordre d’acquisition des coupes en fonction du facteur multiband) qui peut être calculé a l’intérieur du script avec un appel à une fonction matlab. Pas très facile à configurer, je vous conseille d’utiliser le fichier slspec.txt qui doit être placé dans le même répertoire que le script et d’où vous lancerez le script.

Les étapes de pré-traitement placées dans mon script:

- denoise
- deGibbs
- correction topup/eddy + eddyQC
- création du masque du cerveau
- correction du biais
- smoothing


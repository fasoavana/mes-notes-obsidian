
framwork de gestion et d'automation de tache dzblrepr pzt mivtodog, il vomninr unr inyrtgzvr rn lihgnr fr vommzn,fr , un langege de script , et une infrastructurede gestion . powershell est concu pour gerer et autromatiser les taches administratives sur les systeme d'exploitation wordow , mais il est egalement disponibler sur macOS et Linux
powershell est construit sur . NET , ce qui lui permet d'utiliser des  objets . NET  directement dans le shell

mode de fonciotnnment
 pwerxwhell foncitonne selon une apporche oroen,tée objet et utilise une pipleine pour traiter les donnée. voici les principes  de base de son fonciotnnment:
 . Cmdlets : lescmdlets sont des commmandes spécicifgique integréé dans pwershell . elle suivbent un format standr verbe-nom(- par exempl;e, get-Procetss, Set-service)
. Pipleline : Powershell utilsie un pipleine ce qqui perment de asser la sortie d'une commande comme entrée à une autre commande . par exemple: 
Get-Process | where-Objet {$_.CPU -gt 50}

Get-Pocess récupere les prcessur oen cours et where objet firltre ceux qui utilisent plus de  50% de CPU. Le pipleline facilite la manipulaton et le filtrage des donné.
. Objets : contrairement  aux autres shells qui renvoient de texte bruit Powershell  renvoie des Objetsw . Cela signifie qui chaque sortie est dstruicturé et pssède des proprietée et des methodes ce qui facilitre l'interaction avec les donnée
. Modules : Les modules
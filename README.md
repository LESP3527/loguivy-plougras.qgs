# Carte QGIS — Loguivy-Plougras (22131, Côtes-d'Armor)

Projet QGIS prêt à l'emploi : **`loguivy-plougras.qgs`**

## Contenu du projet

| Couche | Source | Contenu |
|---|---|---|
| Lieux-dits (BD TOPO) | IGN — Géoplateforme (WFS) | 170 lieux-dits et hameaux nommés (toponyme dans le champ `toponyme`) |
| Voirie (BD TOPO) | IGN — Géoplateforme (WFS) | Tronçons de routes, rues et chemins de la commune |
| Sections cadastrales (dérivées) | calculée dans le projet | Contours des sections, agrégés depuis les parcelles |
| Parcelles (cadastre) | IGN — Parcellaire Express (WFS) | 8 826 parcelles (n° dans le champ `numero`, surface dans `contenance`) |
| Contour de la commune | IGN — Parcellaire Express (WFS) | Limite officielle de la commune |
| Orthophoto IGN | IGN — Géoplateforme (WMTS) | Photos aériennes (désactivée par défaut : cocher la couche) |
| Fond OpenStreetMap | OpenStreetMap (XYZ) | Fond de carte général |

Système de coordonnées du projet : **RGF93 / Lambert-93 (EPSG:2154)**.

## Ouverture

1. Lancez QGIS 3.x (version LTR 3.34 ou plus récente recommandée).
2. `Projet ▸ Ouvrir depuis… ▸ Fichier` et choisissez `loguivy-plougras.qgs`
   (ou double-cliquez sur le fichier).
3. Une connexion Internet est **nécessaire** : les données sont chargées en direct
   depuis les serveurs de l'IGN (Géoplateforme) et d'OpenStreetMap. Elles sont donc
   toujours à jour.
4. Au premier affichage, faites un clic droit sur la couche **« Contour de la commune »**
   ▸ *Zoomer sur la(les) couche(s)* pour centrer la vue sur Loguivy-Plougras.

## Personnalisation rapide

- **Étiquettes des parcelles** : double-clic sur la couche ▸ onglet *Étiquettes* ▸
  *Étiquette simple* ▸ champ **numero** (conseillé : visible à partir du 1:5 000).
- **Étiquettes des lieux-dits** : même manipulation avec le champ **toponyme**.
- **Fond de plan cadastral image** : ajoutez le WMS
  `https://data.geopf.fr/wms/ows` couche `CADASTRALPARCELS.PARCELLAIRE_EXPRESS`
  si vous voulez le plan cadastral « image » officiel.

## Si une couche ne se charge pas

Ajoutez-la manuellement : `Couche ▸ Ajouter une couche ▸ Ajouter une couche WFS / OGC API-Features`,
nouvelle connexion avec l'URL :

```
https://data.geopf.fr/wfs/ows
```

Puis choisissez la couche et, dans les propriétés de la couche ▸ *Source* ▸
*Filtre de fournisseur (requête)*, saisissez :

- Parcelles : `code_insee = '22131'`
- Contour : `code_insee = '22131'`
- Voirie : `insee_commune_gauche = '22131' OR insee_commune_droite = '22131'`
- Lieux-dits : `insee_commune = '22131'`

La couche « Sections cadastrales (dérivées) » est une couche virtuelle calculée à
partir des parcelles : elle s'affiche une fois les parcelles chargées. Si elle
signale une erreur, ouvrez ses propriétés ▸ *Source* et vérifiez la requête :
`SELECT section, ST_Union(geometry) AS geometry FROM parcelles_22131 GROUP BY section`.

## Sources et licences

- Cadastre (Parcellaire Express) et BD TOPO : © IGN — [Géoplateforme](https://www.geoplateforme.fr/), licence ouverte (Etalab 2.0).
- Fond OpenStreetMap : © les contributeurs d'[OpenStreetMap](https://www.openstreetmap.org/copyright), licence ODbL.

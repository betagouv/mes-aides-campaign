# Version statique de mes-aides pour la campagne de communication

## Stratégie d'implémentation

1. Dump statique de la page.
2. Mise en ligne sur simuler.mes-aides.gouv.fr.
3. Réécriture des liens sortants pour qu’ils pointent en absolu vers mes-aides.gouv.fr/.
4. Ajout d’une bannière de cookies.
5. Ajout du traceur au clic sur “j’accepte les cookies”.
6. Réécriture du code piwik pour que les visites soient enregistrées comme une visite sur la page d’accueil depuis la campagne “communication-nationale”, source “tracker-homepage".
    * pk_campaign
    * pk_kwd
7. Ajout du traceur au clic sur n’importe quel lien sortant (donc avec temporisation).
71. Cross browser => IE10 via Sourcelabs cf. github issue
72. Rediriger `www.simuler.` vers `simuler.`
8. Support SSL.
81. onload sur l'iframe
82. Bannière en bas
83. Spinner https://github.com/sgmap/mes-aides-ui/pull/471/commits/948deb86f2be3b4bd2b0995fa29c8c7eb6876a1a
84. Pas de bannière au retour via localstorage
    * Ajout minimal
9. Ajout de `prefetch` sur les assets de mes-aides.
    * voir prefetch et preload et prerender
10. Exploitation des assets de mes-aides qui ne sont pas versionnés.



## Notes pour l'implémentation


```
document.querySelectorAll('a').forEach(a => {
    a.addEventListener('click', event => {
        event.preventDefault()
        trackVisit()
        showSpinner()
        window.setTimeout(() => { document.location = a.href }, 1500)
    }
})
```

### Envoi de la configuration NGINX

```
scp mes-aides-campaign.conf root@mes-aides.gouv.fr:/etc/nginx/conf.d/
ssh root@mes-aides.gouv.fr "service nginx reload"
```


### Mise à jour des fichiers statiques

```
rsync -r --exclude=*.conf --exclude=readme.md --exclude=.git --exclude=.DS_Store . root@mes-aides.gouv.fr:/var/tmp/simuler/

rsync -v -r --exclude=*.conf --exclude=readme.md --exclude=.git --exclude=.DS_Store --exclude=*.js --exclude=tracker*.html . root@mes-aides.gouv.fr:/var/tmp/simuler/
```

### Initial SSL certificate request via Certbox

```./certbot-auto certonly --webroot -w /var/tmp/simuler/ -d simuler.mes-aides.gouv.fr```

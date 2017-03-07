# Version statique de mes-aides pour la campagne de communication

## Stratégie d'implémentation

1. Dump statique de la page.
2. Mise en ligne sur simuler.mes-aides.gouv.fr.
3. Réécriture des liens sortants pour qu’ils pointent en absolu vers mes-aides.gouv.fr/.
4. Ajout d’une bannière de cookies.
5. Ajout du traceur au clic sur “j’accepte les cookies”.
6. Réécriture du code piwik pour que les visites soient enregistrées comme une visite sur la page d’accueil depuis la campagne “communication-nationale”, source “tracker-homepage".
7. Ajout du traceur au clic sur n’importe quel lien sortant (donc avec temporisation).
8. Support SSL.
9. Ajout de `prefetch` sur les assets de mes-aides.
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
    }
) 
```

### Mise à jour des fichiers statiques

rsync -r --exclude=*.conf --exclude=readme.md --exclude=.git --exclude=.DS_Store . root@mes-aides.gouv.fr:/var/tmp/simuler/


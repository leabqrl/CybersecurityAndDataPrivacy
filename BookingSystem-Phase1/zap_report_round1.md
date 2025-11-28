# ZAP by Checkmarx Scanning Report

ZAP by [Checkmarx](https://checkmarx.com/).


## Summary of Alerts

| Niveau de risque | Number of Alerts |
| --- | --- |
| Haut | 1 |
| Moyen | 3 |
| Faible | 2 |
| Pour information | 0 |




## Alertes

| Nom | Niveau de risque | Number of Instances |
| --- | --- | --- |
| Injection SQL | Haut | 1 |
| Absence de Jetons Anti-CSRF | Moyen | 1 |
| Content Security Policy (CSP) Header Not Set | Moyen | 2 |
| Missing Anti-clickjacking Header | Moyen | 2 |
| Application Error Disclosure | Faible | 1 |
| X-Content-Type-Options Header Missing | Faible | 5 |




## Alert Detail



### [ Injection SQL ](https://www.zaproxy.org/docs/alerts/40018/)



##### Haut (Moyen)

### Description

SQL injection may be possible.

* URL: http://localhost:8000/register

  * Méthode: `POST`
  * Parameter: `username`
  * Attaquer: `foo-bar@example.com AND 1=1 -- `
  * Evidence: ``
  * Other Info: `The page results were successfully manipulated using the boolean conditions [foo-bar@example.com AND 1=1 -- ] and [foo-bar@example.com AND 1=2 -- ]
The parameter value being modified was stripped from the HTML output for the purposes of the comparison.
Data was returned for the original parameter.
The vulnerability was detected by successfully restricting the data originally returned, by manipulating the parameter.`


Instances: 1

### Solution

Do not trust client side input, even if there is client side validation in place.
In general, type check all data on the server side.
If the application uses JDBC, use PreparedStatement or CallableStatement, with parameters passed by '?'
If the application uses ASP, use ADO Command Objects with strong type checking and parameterized queries.
If database Stored Procedures can be used, use them.
Do *not* concatenate strings into queries in the stored procedure, or use 'exec', 'exec immediate', or equivalent functionality!
Do not create dynamic SQL queries using simple string concatenation.
Escape all data received from the client.
Apply an 'allow list' of allowed characters, or a 'deny list' of disallowed characters in user input.
Apply the principle of least privilege by using the least privileged database user possible.
In particular, avoid using the 'sa' or 'db-owner' database users. This does not eliminate SQL injection, but minimizes its impact.
Grant the minimum database access that is necessary for the application.

### Reference


* [ https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html ](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)


#### CWE Id: [ 89 ](https://cwe.mitre.org/data/definitions/89.html)


#### WASC Id: 19

#### Source ID: 1

### [ Absence de Jetons Anti-CSRF ](https://www.zaproxy.org/docs/alerts/10202/)



##### Moyen (Faible)

### Description

Aucun jetons Anti-CSRF n'ont été trouvés dans un formulaire HTML.
La contrefaçon de requête intersites (Cross Site Request Forgery - CSRF) est une attaque qui consiste à forcer une victime à envoyer une requête HTTP vers une destination cible, sans qu'elle n'en aie ni connaissance ni intention, afin d'effectuer une action en se faisant passer pour la victime. La cause originelle est que les fonctionnalités de l'application sont appelées à l'aide d'URL ou d'actions de formulaires prévisibles et reproductibles. La nature de l'attaque est que le CSRF exploite la confiance qu'un site internet accorde à un utilisateur. En revanche, le cross-site scripting (XSS) exploite la confiance que l'utilisateur porte à un site internet. Comme XSS, les attaques CSRF ne sont pas nécessairement multi-sites, mais elles peuvent l'être. La contrefaçon de requête intersite est également connue sous les noms CSRF, XSRF, attaque en un clic (one-click attack), session riding, confused deputy et sea surf.

Les attaques CSRF sont efficaces dans de nombreuses situations, notamment:
* quand la victime a une session active sur le site cible.
    * quand la victime est authentifiée via HTTP auth sur le site cible.
    * quand la victime est sur le même réseau local que le site cible.

CSRF a d'abord été utilisée pour effectuer une action contre un site cible en utilisant les privilèges de la victime, mais des techniques récentes permettent d'avoir accès à des renseignements en accédant à la réponse. Le risque de divulgation d'informations est considérablement augmenté lorsque le site cible est vulnérable aux XSS, parce que XSS peut être utilisé comme une plateforme pour CSRF, permettant à l'attaque d'opérer dans les limites de la politique de même origine.

* URL: http://localhost:8000/register

  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: `<form action="/register" method="POST">`
  * Other Info: `Aucun jetons Anti-CSRF connus [anticsrf, CSRFToken, __RequestVerificationToken, csrfmiddlewaretoken, authenticity_token, OWASP_CSRFTOKEN, anoncsrf, csrf_token, _csrf, _csrfSecret, __csrf_magic, CSRF, _token, _csrf_token, _csrfToken] n'a été trouvé dans le formulaire HTML suivant: [Form 1: "birthdate" "password" "username" ].  `


Instances: 1

### Solution

Phase: Architecture et Design
Utilisez une librairie ou un framework approuvé qui ne permet pas cette vulnérabilité, ou qui contient des fonctionnalités permettant d'éviter plus facilement cette vulnérabilité.
Utilisez  par exemple des librairies anti-CSRF telles que CSRFGuard de l'OWASP.

Phase: Implémentation
Assurez-vous que votre application soit exempte de problèmes de cross-site scripting, parce que la plupart des défenses contre le CSRF peuvent être contournées en utilisant des scripts contrôlés par le pirate.

Phase: Architecture et Design
Générez une valeur à usage unique pour chaque formulaire, placez-la dans le formulaire et vérifiez-la à la réception du formulaire. Assurez-vous que cette valeur unique ne soit pas prévisible (CWE-330).
Notez que ceci peut aussi être contourné en utilisant XSS.

Identifiez les opérations particulièrement dangereuses. Quand l'utilisateur exécute une opération dangereuse, envoyez une requête de confirmation distincte pour vérifier que l'utilisateur veut effectivement effectuer cette opération.
Notez que ceci peut aussi être contourné en utilisant XSS.

Utilisez la librairie de gestion de session ESAPI.
Cette librairie comprend un composant pour le contrôle de CSRF.

N'utilisez pas la méthode GET pour les requêtes entraînant un changement d'état.

Phase: Implémentation
Vérifiez l'en-tête HTTP Referer pour voir si la requête provient d'une page attendue. Ceci pourrait toutefois restreindre la fonctionnalité de l'application, car les utilisateurs ou les serveurs proxy pourraient avoir désactivé le renvoi du HTTP Referer pour des raisons de confidentialité.

### Reference


* [ https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html ](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
* [ https://cwe.mitre.org/data/definitions/352.html ](https://cwe.mitre.org/data/definitions/352.html)


#### CWE Id: [ 352 ](https://cwe.mitre.org/data/definitions/352.html)


#### WASC Id: 9

#### Source ID: 3

### [ Content Security Policy (CSP) Header Not Set ](https://www.zaproxy.org/docs/alerts/10038/)



##### Moyen (Haut)

### Description

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. CSP provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.

* URL: http://localhost:8000/

  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register

  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``


Instances: 2

### Solution

Ensure that your web server, application server, load balancer, etc. is configured to set the Content-Security-Policy header.

### Reference


* [ https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
* [ https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html ](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
* [ https://www.w3.org/TR/CSP/ ](https://www.w3.org/TR/CSP/)
* [ https://w3c.github.io/webappsec-csp/ ](https://w3c.github.io/webappsec-csp/)
* [ https://web.dev/articles/csp ](https://web.dev/articles/csp)
* [ https://caniuse.com/#feat=contentsecuritypolicy ](https://caniuse.com/#feat=contentsecuritypolicy)
* [ https://content-security-policy.com/ ](https://content-security-policy.com/)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3

### [ Missing Anti-clickjacking Header ](https://www.zaproxy.org/docs/alerts/10020/)



##### Moyen (Moyen)

### Description

The response does not protect against 'ClickJacking' attacks. It should include either Content-Security-Policy with 'frame-ancestors' directive or X-Frame-Options.

* URL: http://localhost:8000/

  * Méthode: `GET`
  * Parameter: `x-frame-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register

  * Méthode: `GET`
  * Parameter: `x-frame-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``


Instances: 2

### Solution

Modern Web browsers support the Content-Security-Policy and X-Frame-Options HTTP headers. Ensure one of them is set on all web pages returned by your site/app.
If you expect the page to be framed only by pages on your server (e.g. it's part of a FRAMESET) then you'll want to use SAMEORIGIN, otherwise if you never expect the page to be framed, you should use DENY. Alternatively consider implementing Content Security Policy's "frame-ancestors" directive.

### Reference


* [ https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Frame-Options ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Frame-Options)


#### CWE Id: [ 1021 ](https://cwe.mitre.org/data/definitions/1021.html)


#### WASC Id: 15

#### Source ID: 3

### [ Application Error Disclosure ](https://www.zaproxy.org/docs/alerts/90022/)



##### Faible (Moyen)

### Description

This page contains an error/warning message that may disclose sensitive information like the location of the file that produced the unhandled exception. This information can be used to launch further attacks against the web application. The alert could be a false positive if the error message is found inside a documentation page.

* URL: http://localhost:8000/register

  * Méthode: `POST`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: `HTTP/1.1 500 Internal Server Error`
  * Other Info: ``


Instances: 1

### Solution

Review the source code of this page. Implement custom error pages. Consider implementing a mechanism to provide a unique error reference/identifier to the client (browser) while logging the details on the server side and not exposing them to the user.

### Reference



#### CWE Id: [ 550 ](https://cwe.mitre.org/data/definitions/550.html)


#### WASC Id: 13

#### Source ID: 3

### [ X-Content-Type-Options Header Missing ](https://www.zaproxy.org/docs/alerts/10021/)



##### Faible (Moyen)

### Description

The Anti-MIME-Sniffing header X-Content-Type-Options was not set to 'nosniff'. This allows older versions of Internet Explorer and Chrome to perform MIME-sniffing on the response body, potentially causing the response body to be interpreted and displayed as a content type other than the declared content type. Current (early 2014) and legacy versions of Firefox will use the declared content type (if one is set), rather than performing MIME-sniffing.

* URL: http://localhost:8000/

  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`
* URL: http://localhost:8000/register

  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`
* URL: http://localhost:8000/static/footer.js

  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`
* URL: http://localhost:8000/static/index.js

  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`
* URL: http://localhost:8000/static/tailwind.css

  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`


Instances: 5

### Solution

Ensure that the application/web server sets the Content-Type header appropriately, and that it sets the X-Content-Type-Options header to 'nosniff' for all web pages.
If possible, ensure that the end user uses a standards-compliant and modern web browser that does not perform MIME-sniffing at all, or that can be directed by the web application/web server to not perform MIME-sniffing.

### Reference


* [ https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/compatibility/gg622941(v=vs.85) ](https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/compatibility/gg622941(v=vs.85))
* [ https://owasp.org/www-community/Security_Headers ](https://owasp.org/www-community/Security_Headers)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3



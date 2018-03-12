
# setup-wordpress-lightsail
How setup a Wordpress on AWS Lightsail


1. Create new instance on AWS Lightsail
2. How disable Bitnami banner ? You can check there : https://docs.bitnami.com/general/components/bninfo/
3. Create Static IP
4. Create DNS zone or Create rule A in DNS to point static IP
5. After Domain is setup, Aller sur le site avec le domain pour migrer avec All-in One migration
6. Create SSL Certificat avec Letsencrypt : https://certbot.eff.org/
  `folder du wordpress pour le certificat : /home/bitnami/apps/wordpress/htdocs`
7. Edit Apache2 bitnami config pour setup le certificat de Letsencrypt : 
 `sudo nano /home/bitnami/stack/apache2/conf/bitnami/bitnami.conf`
8. Ligne a changer ou a ajouter : 
```
SSLCertificateFile "/etc/letsencrypt/live/DOMAIN.COM/cert.pem"
SSLCertificateKeyFile "/etc/letsencrypt/live/DOMAIN.COM/privkey.pem"
SSLCertificateChainFile "/etc/letsencrypt/live/DOMAIN.COM/fullchain.pem"
```
9. Activer l'auto renew de certbit si pas deja fait
10. setup le HTTPS only de Wordpress : sudo nano /opt/bitnami/apps/wordpress/conf/httpd-prefix.conf
11. Ajouter ceci en haut de `httpd.prefix.conf` : 
```
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^/(.*) https://www.CHANGE_ME_WITH_DOMAIN.COM/$1 [R,L]
```
12. Restart apache, httpd... : `sudo /opt/bitnami/ctlscript.sh restart`
13. Aller sur le site pour voir que le HTTPS fonctionne bien
14. Installer le plugin WP SMTP Mail parce que les instance Lightsail ne peux send de email.
15. Aller dans setting -> WP SMTP Mail pour configurer. Tuto de Bitnami -> https://docs.bitnami.com/aws/apps/wordpress/#how-to-configure-outbound-email-settings
16. Sélectionner de quel email vous voulez envoyer les demande via formulaire et sélectionner -> Other SMTP
> Si vous utiliser gmail ou google comme provider de email oublier pas d'accorder l'accès au compte pour les applications moins sécurisées : https://support.google.com/accounts/answer/6010255?hl=fr

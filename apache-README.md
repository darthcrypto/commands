# commands for apache setups

### setup apache with http listening for different sites on same port

Listen 443

<VirtualHost *:443>
    DocumentRoot /var/www/html
    ServerName developer1.dev.net
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/developer1.dev.net.crt
    SSLCertificateKeyFile /etc/pki/tls/private/developer1.dev.net.key
</VirtualHost>

<Directory "/var/www/special">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>

<VirtualHost *:80>
  ServerAdmin webmaster@developer1.dev.net
  DocumentRoot /var/www/special
  ServerName big.dev.net
  ErrorLog logs/domain11.dev.net-error_log
  TransferLog logs/domain11.dev.net-access_log
</VirtualHost>

<VirtualHost *:80>
  DocumentRoot /var/www/html
  ServerName developer1.dev.net
</VirtualHost>

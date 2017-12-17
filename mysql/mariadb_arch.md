# [Archlinux]
`pacman -S  mariadb libmariadbclient mariadb-clients mysql-workbench`

`mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql`

`mysql_secure_installation`

## Set root password mysql
`mysqld_safe --skip-grant-tables &`

`mysql -u root`

`use mysql;`

`update user set password=PASSWORD("mynewpassword") where User='root';`

`update user set plugin="mysql_native_password";`

`quit;`

# [Debian Server]
@TODO :(

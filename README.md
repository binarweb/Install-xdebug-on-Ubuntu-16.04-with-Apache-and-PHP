# Installing Xdebug and Webgrind on Ubuntu 16.04 with Apache and PHP

This tutorial will help you install xdebug and webgrind profiling. PHP is NOT running as PHP-FPM.  

## Install dependencies

```bash
apt update
apt install php-dev
```

## Download and compile Xdebug

```bash
cd ~
git clone https://github.com/xdebug/xdebug.git
cd xdebug
./rebuild.sh
```
**Notes:**  
- the `./rebuild.sh` shell file is running `phpize`, `./configure --enable-xdebug`, `make` and `make install` in the background.  
- in my case, the `xdebug.so` file was installed in `/usr/lib/php/20151012/` folder  

## Install Xdebug

```bash
echo "extension=xdebug.so" > /etc/php/7.0/mods-available/xdebug.ini
ln -s /etc/php/7.0/mods-available/xdebug.ini /etc/php/7.0/apache2/conf.d/xdebug.ini
service apache2 restart
```

To test the installation, create a php file and put `phpinfo` in it. In the output there should be xdebug.  

## Enabling Xdebug

In .htaccess file, paste the following:

```text
php_value xdebug.profiler_enable 1
```

Now, on every request Xdebug will create a `cachegrind.out.` in `/tmp` folder that can be read by Webgrind.  
Additional information about the configuration can be found on https://xdebug.org/docs/profiler.  

## Install Webgrind

```bash
cd /var/www/html
wget https://github.com/jokkedk/webgrind/archive/v1.6.0.zip
unzip v1.6.0.zip
```

Now you can access Webgrind from the browser by accessing `http://<your_ip>/webgrind-1.6.0/`  

### Rerences:
https://github.com/xdebug/xdebug  
https://github.com/jokkedk/webgrind  

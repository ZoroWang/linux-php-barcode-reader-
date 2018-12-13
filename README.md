# PHP Barcode Reader for Linux

The sample demonstrates how to make PHP barcode reader on Ubuntu 16.04 with **PHP7** and [Dynamsoft Barcode Reader for Linux](https://www.dynamsoft.com/Products/barcode-reader-c-api-linux.aspx).

## License
Get the [trial license](https://www.dynamsoft.com/CustomerPortal/Portal/Triallicense.aspx).

## Installation
* php7.0-cli, php7.0-dev, libxml2-dev

    ``` 
    sudo apt-get install php7.0-cli php7.0-dev libxml2-dev
    ```
* [php-7.0.29](http://php.net/get/php-7.0.29.tar.gz/from/a/mirror) source code 
* [Dynamsoft Barcode Reader SDK](https://www.dynamsoft.com/Downloads/Dynamic-Barcode-Reader-Download.aspx?edition=linux)

    ```
    tar xzf dbr-linux-6.3.0.tar.gz
    sudo cp Dynamsoft/BarcodeReader6.3/lib/libDynam* /usr/lib
    ```

## How to Build the Extension
1. Unzip PHP source code and then change directory to **ext**:

    ```
    tar -xzf php-7.0.29.tar.gz
    cd ~/php-7.0.29/ext/
    ```

2. Create an extension folder:

    ```
    ./ext_skel --extname=dbr
    cd dbr
    ```

3. Set a valid SDK license in **dbr.c**:
    
   ```
   DBR_InitLicense(hBarcode, "t0068NQAAAIY/7KegDlZn7YiPdAj0cbA11n2CwuCEWnk2KYla55ozdfmoasjRIpHhl0EUZmko/zxfxFLH3FpLw694uihoCVM=");
   ```

4. Copy **config.m4** and **dbr.c** to **~/php-7.0.29/ext/dbr**:
5. Copy **php.ini** to **/etc/php/7.0/cli/php.ini**.
6. Build and install the extension:
    
    ```
    phpize
    ./configure
    make
    sudo make install
    ```
6. Change the input file in **reader.php**:

    ```php
    $filename = "/home/xiao/AllSupportedBarcodeTypes.tif";
    ```

7. Run **reader.php**:
    
    ```
    php -c /etc/php/7.0/cli/php.ini reader.php
    ```
    
    ![php barcode reader](https://www.codepool.biz/wp-content/uploads/images/php-dbr6.3.PNG)


## Online Barcode Reader with PHP Custom Extension
Get the [source code][3] and deploy it to any web server.
![php online barcode reader](http://www.codepool.biz/wp-content/uploads/images/php-dbr-online.PNG)

### PHP on Apache
1. Install **php7.0-curl**, **apache2** and **libapache2-mod-php7.0**:

    ```
    sudo apt-get install php7.0-curl apache2 libapache2-mod-php7.0
    ```
2. Add extension to `/etc/php/7.0/apache2/php.ini `:

    ```
    extension=dbr.so
    ```
3. Get write permissions:

    ```
    sudo chgrp -R www-data /var/www/html/reader
    sudo chmod -R g+rw /var/www/html/reader
    ```
4. Start **Apache**:
    
    ```
    sudo service apache2 start
    #sudo service apache2 stop // if you want to stop Apache
    ```
5. Visit `http://localhost/reader/index.php`.

### PHP on Nginx
1. Install **Nginx** and **php7.0-cgi**:

    ```
    sudo apt-get install nginx php7.0-cgi
    ```
2. Enable PHP in configuration file:
    
    ```
    sudo vi /etc/nginx/sites-available/default

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

    location ~ \.php$ {
            include snippets/fastcgi-php.conf;

            # With php7.0-cgi alone:
            fastcgi_pass 127.0.0.1:9000;
            # With php7.0-fpm:
            #fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
    ```

3. Get write permissions:
    
    ```
    sudo chgrp -R www-data /usr/share/nginx/html/reader
    sudo chmod -R g+rw /usr/share/nginx/html/reader
    ```
4. Run **Nginx** and **php-cgi**:
    
    ```
    sudo nginx
    # sudo nginx –s stop // if you want to stop Nginx
    sudo php-cgi -b 127.0.0.1:9000 -c /etc/php/7.0/cli/php.ini
    ```
5. Visit `http://localhost/reader/index.php`.

## Blog
* [Updating PHP Barcode Extension for PHP7](http://www.codepool.biz/php-barcode-linux-ubuntu-php7.html)
* [How to Make PHP Barcode Reader on Linux][4]
* [How to Deploy PHP Code to Apache and Nginx on Ubuntu 14.04][5]

[1]:http://labs.dynamsoft.com/linux-barcode-reader-overview.htm
[2]:http://php.net/downloads.php
[3]:https://github.com/dynamsoftlabs/linux-php-barcode-reader-/tree/master/reader
[4]:http://www.codepool.biz/linux-php-barcode-reader.html
[5]:http://www.codepool.biz/deploy-php-nginx-apache-ubuntu.html


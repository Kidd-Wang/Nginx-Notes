Save the configuration below as nginx-plus-api.conf under /etc/nginx/conf.d/
 
    server {
    # Conventional port for the NGINX Plus API is 8080
    listen 8080;

    # Uncomment to use HTTP Basic authentication; see (3) above
    #auth_basic "NGINX Plus API";
    #auth_basic_user_file /etc/nginx/users;

    # Uncomment to use permissions based on IP address; see (4) above
    #allow 10.0.0.0/8;
    #deny all;

    # Conventional location for accessing the NGINX Plus API 
    location /api/ {
        # Enable in read-write mode
        api write=on;

        # Uncomment to further restrict write permissions; see note above
        #limit_except GET {
            #auth_basic "NGINX Plus API";
            #auth_basic_user_file /etc/nginx/admins;
        #}
    }

    # Conventional location of the NGINX Plus dashboard
    location = /dashboard.html {
        root /usr/share/nginx/html;
    }

    # Redirect requests for "/" to "/dashboard.html"
    location / {
        return 301 /dashboard.html;
    }

    # Redirect requests for pre-R14 dashboard
    location /status.html {
        return 301 /dashboard.html;
    }
    }
   


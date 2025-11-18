# Installation Guide for arIiifPlugin

## Prerequisites

Before installing the arIiifPlugin, ensure you have:

1. **AtoM Installation**
   - AtoM 2.6 or higher
   - PHP 7.2+ (PHP 8.3 compatible)
   - Symfony 1.4 framework

2. **Cantaloupe IIIF Image Server**
   - Version 5.0.6 or higher
   - Running on port 8182 (default)
   - Properly configured delegates.rb

3. **Server Requirements**
   - Ubuntu 20.04+ or similar Linux distribution
   - Nginx or Apache web server
   - SSL certificate (for HTTPS)
   - At least 2GB RAM for image processing

## Installation Steps

### Step 1: Download the Plugin
```bash
# Navigate to AtoM plugins directory
cd /usr/share/nginx/atom/plugins

# Clone the repository
git clone https://github.com/yourusername/arIiifPlugin.git

# Or download and extract the archive
wget https://github.com/yourusername/arIiifPlugin/archive/main.zip
unzip main.zip
mv arIiifPlugin-main arIiifPlugin
rm main.zip
```

### Step 2: Set Permissions
```bash
# Set ownership to web server user
sudo chown -R www-data:www-data arIiifPlugin/

# Set proper permissions
sudo chmod -R 755 arIiifPlugin/
```

### Step 3: Configure Cantaloupe

Edit `/opt/cantaloupe-5.0.6/cantaloupe.properties`:
```properties
# Enable slash substitute
slash_substitute = _SL_

# Set max pixels (adjust based on your needs)
max_pixels = 50000000

# Enable delegate script
delegate_script.enabled = true
delegate_script.pathname = /opt/cantaloupe-5.0.6/delegates.rb

# Set source
source.delegate = true
```

### Step 4: Configure the Plugin

Edit `/usr/share/nginx/atom/plugins/arIiifPlugin/config/app.yml`:
```yaml
all:
  iiif:
    base_url: https://yourdomain.com/iiif/2
    api_version: 2
    server_type: cantaloupe
    enable_manifests: true
```

### Step 5: Configure Web Server

#### For Nginx

Add to your site configuration:
```nginx
# IIIF proxy to Cantaloupe
location /iiif/ {
    proxy_pass http://127.0.0.1:8182/iiif/;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $host;
    
    # CORS headers
    add_header 'Access-Control-Allow-Origin' '*' always;
    add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS' always;
    
    # Timeouts for large images
    proxy_connect_timeout 300s;
    proxy_send_timeout 300s;
    proxy_read_timeout 300s;
}
```

### Step 6: Clear Cache and Restart Services
```bash
# Clear AtoM cache
cd /usr/share/nginx/atom
sudo -u www-data php symfony cc

# Restart web server
sudo systemctl restart nginx

# Restart Cantaloupe
sudo systemctl restart cantaloupe
```

## Troubleshooting

See README.md for common issues and solutions.

## Support

Contact: johan@theahg.co.za
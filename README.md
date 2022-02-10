## openresty-geoip2
文档和这个一致，只在上面加了geoip2的模块 
https://github.com/openresty/docker-openresty

## geoip2的使用方法
```
##
# GeoIP
## nginx配置文件里的http模块下定义如下代码
http{
    geoip2 /usr/share/GeoIP/GeoLite2-Country.mmdb {
        auto_reload 5m;
        $geoip2_metadata_country_build metadata build_epoch;
        $geoip2_data_country_name country names en;
        $geoip2_data_country_code country iso_code;
    }
    
    geoip2 /usr/share/GeoIP/GeoLite2-City.mmdb {
        auto_reload 5m;
        $geoip2_continent_code continent code;
        $geoip2_country country names en;
        $geoip2_country_code country iso_code;
        $geoip2_region subdivisions 0 names en;
        $geoip2_region_code subdivisions 0 iso_code;
        $geoip2_city city names en;
        $geoip2_postal_code postal code;
        $geoip2_latitude location latitude;
        $geoip2_longitude location longitude;
        $geoip2_timezone location time_zone;
    }
    
    geoip2 /usr/share/GeoIP/GeoLite2-ASN.mmdb {
        auto_reload 5m;
        $geoip2_asn autonomous_system_number;
        $geoip2_organization autonomous_system_organization;
    }
}

## 使用 在server模块里可以这样用
server {
        listen       80;
        server_name  test.test.com;
        add_header x-geoip-continent_code $geoip2_continent_code;
        add_header x-geoip-country $geoip2_country;
        add_header x-geoip-country_code $geoip2_country_code;
        add_header x-geoip-region $geoip2_region;
        add_header x-geoip-region_code $geoip2_region_code;
        add_header x-geoip-city $geoip2_city;
        add_header x-geoip-postal_code $geoip2_postal_code;
        add_header x-geoip-latitude $geoip2_latitude;
        add_header x-geoip-longitude $geoip2_longitude;
        add_header x-geoip-timezone $geoip2_timezone;

        if ($geoip2_city != ''){
            return 200 "$geoip2_city";
        }
}
```
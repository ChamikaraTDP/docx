# Daily Commands

## audio mixer
> alsamixer

## list wifi
>  nmcli device wifi list
>  nmcli device wifi connect [id]

## run pgadmin
>  source pgadmin4/bin/activate
> pgadmin4

## view pg servers
> pg_lsclusters

## login to psql
> sudo -u postgres psql

## install deb file
> sudo dpkg -i [path_to_package]

## restart service
> sudo systemctl restart [service]

## imagemagick

convert Sujathas-Anthurium.png -fill Gray50 -colorize 70 -resize 70% miff:- | composite -dissolve 50 -gravity southeast -geometry +20+20 - IMG_0937.JPG IMG_0937_diss_gray_8.JPG

convert  IMG_0747.JPG  -scale 50% -set filename:f '%t_minified.%e' +adjoin '%[filename:f]'

for image in *.JPG 
do 
 composite -dissolve 50 -gravity southeast -geometry +20+20  watermark.png "$image" "$image"_watermarked.JPG
done



for image in *.JPG 
do 
  convert "$image" -resize 1920x1280 miff:- | composite -dissolve 50 -gravity southeast -geometry +20+20  watermark.png - "$image"_watermarked.JPG
done




for dir in ./*;                           
do
	for img in "$dir"/*.JPG;
	do 
		echo "hello img $img";
	done
done


## Rclone 

rclone copyto remote:"2024.04.07 Inter 18 - 04.mp4" /home/chamikara/Downloads/nokia-firmware/2024.04.07-Inter-18-04.mp4  --progress

## SSH to aws

ssh -i /home/chamikara/Documents/secrets/LightsailDefaultKey-ap-southeast-1.pem admin@54.151.140.126


## edit symlink
ln -sf php7.2 php


## nginx folder path
/usr/share/nginx

## nginx config path
/etc/nginx


# Certbot

## install certbot

### instructions
https://certbot.eff.org/instructions?ws=nginx&os=snap&tab=standard

sudo snap install --classic certbot

## install certificate
Run and follow the instructions

sudo certbot --nginx







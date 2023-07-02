
```sh
gource --camera-mode track --logo ../site-contribuer/content/theme/assets/images/geotribu/geotribu_logo_tipi_seul_carre.png --title "Site Geotribu - 2022" --date-format "%e %B" --seconds-per-day 1 --auto-skip-seconds 1 --key --start-date 2022-01-01 --stop-date 2022-12-31 --hide mouse -f -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -crf 1 -threads 0 -bf 0 gource.mp4
```

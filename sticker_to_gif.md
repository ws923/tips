# Converting an iOS Sticker to a GIF

These instructions assume you have already created the sticker you want to convert into a GIF and that you have `ffmpeg` installed on a Mac.

1. Add the sticker to a draft message in the Messages app on your Mac
2. On the file in the messages box, right click and select "Copy"
3. Open Finder and paste the file into a working directory; the file name should be a long string of numbers with the file type of heics: `abc-xzy.heics`
4. If you want to use the script below as-is, rename the file to `input.heics`
5. Open the now-renamed file `input.heics` in Preview
6. Turn on the thumbnail view on the left sidebar
7. Select all images in the thumbnail view
8. Click and drag all of the thumbnails as files to your working directory in Finder; there should be several dozen `.tiff` files
9. Open iTerm, `cd` to your working directory
10. Run the following code, changing the frame rate if you want (15 worked for me):

```bash
ffmpeg -y -framerate 15 -start_number 1 -i 'input-%d (dragged).tiff' -filter_complex "[0:v]format=rgba,scale=400:-1:flags=lanczos,split[a][b]; \
                   [a]palettegen=reserve_transparent=1[p]; \
                   [b][p]paletteuse=dither=sierra2_4a:alpha_threshold=128[out]" -map "[out]" -loop 0 out.gif
```

9. Now you have a normal GIF that includes transparency

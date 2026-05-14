# BTh14 Realistische Visualisierungen in der Verkehrs- und Siedlungsplanung

This viewer wasn't built by us — this is a fork of the [PlayCanvas SuperSplat Viewer](https://github.com/playcanvas/supersplat-viewer).

This thesis was written by Michael Kamm and Marco Ammann at the [University of Applied Sciences Northwestern Switzerland (FHNW)](https://www.fhnw.ch).

This Bachelor's thesis aims to display futuristic megaprojects using 3D Gaussian Splatting (3DGS). The project perimeter was captured with a DJI Mini 3 Pro and processed in [Agisoft Metashape](https://www.agisoft.com). The 3DGS scenes are computed using [Jawset Postshot](https://www.jawset.com/).

Our thesis will not be publicly available.

|                    Comparison                     |
| :-----------------------------------------------: |
|                   **Original**                    |
|   <img src="images/Original.jpg" width="800"/>    |
|                     **3DGS**                      |
|     <img src="images/3DGS.png" width="800"/>      |
|        **3DGS with futuristic buildings**         |
| <img src="images/3DGS_Gebaeude.png" width="800"/> |

## Starting the Viewer on a Web Server

To initialize the SuperSplat Viewer on your own server you need a Linux (Ubuntu) based server with Node.js, Git, and NGINX installed. NGINX needs to be fully configured to your liking.

1. Clone the repository:

   ```sh
   git clone https://github.com/MichaelPKamm/BTh14-3DGS-Viewer.git
   cd BTh14-3DGS-Viewer
   ```

2. Install dependencies:

   ```sh
   npm install
   ```

3. Upload the 3DGS file to the server and add it to the public folder:

   ```sh
   scp -P <PORT> <YOUR_LOCAL_PATH>/scene.compressed.ply <USER>@<SERVER_ADDRESS>:/home/<USER>/BTh14-3DGS-Viewer/public/
   ```

   | Placeholder         | Description                                   |
   | ------------------- | --------------------------------------------- |
   | `<PORT>`            | Port of your server                           |
   | `<YOUR_LOCAL_PATH>` | Local path on your laptop to your `.ply` file |
   | `<USER>`            | Your username on the server                   |
   | `<SERVER_ADDRESS>`  | IP address or domain of the server            |

4. Run the build process:

   ```sh
   npm run build
   ```

5. Move the `public` folder into NGINX:

   ```sh
   sudo rsync -av --delete ~/BTh14-3DGS-Viewer/public/ /var/www/3dgsviewer/
   ```

6. Reload the NGINX server:

   ```sh
   sudo systemctl reload nginx
   ```

## Mandatory Files to Run the Viewer on Your Own Server

The `settings.json` file defines the viewpoints (cameras) for the viewer as well as annotations and animations. For examples, please refer to the dedicated appendix of our thesis.

The `scene.compressed.ply` is the 3DGS file that will be displayed by the viewer. Aim for as small a file size as possible.

The `bild.jpg` is the wallpaper image that appears in the background while the 3DGS is loading. Keep in mind that this image will be viewed on many different screen sizes (including phones).

The `favicon.ico` is the favicon displayed next to your website name in the browser tab.

## URL Parameters

The app supports the following URL parameters (note: these are subject to change):

| Parameter   | Description                                                            | Default                  |
| ----------- | ---------------------------------------------------------------------- | ------------------------ |
| `settings`  | URL of the `settings.json` file                                        | `./settings.json`        |
| `content`   | URL of the scene file (`.ply`, `.sog`, `.meta.json`, `.lod-meta.json`) | `./scene.compressed.ply` |
| `skybox`    | URL of an equirectangular skybox image                                 |                          |
| `poster`    | URL of an image to show while loading                                  |                          |
| `noui`      | Hide UI                                                                |                          |
| `noanim`    | Start with animation paused                                            |                          |
| `ministats` | Show runtime CPU/GPU performance graphs                                |                          |
| `unified`   | Force unified rendering mode                                           |                          |
| `aa`        | Enable antialiasing (not supported in unified mode)                    |                          |

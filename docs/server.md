## Setting up Amazon AWS EC2 t2.micro instance

It is pretty straight forward.  

1. Sign up for Amazon AWS Free Tier.  
2. Log into the Amazon AWS Console and go to EC2 Instances.  
3. Create a fresh t2.micro instance. It is a CLI-based server.  
4. Launch the t2.micro instance. You'll be able to see its public IPv4 address.  
5. There is an option to generate an SSH key. The key file will have a `.pem` extension. Download it and keep it safe.  
6. Now that we have our server set up, we can SSH into it. In our terminal, we run:

   ```bash
   ssh -i "t2microKey.pem" ubuntu@ec2-44-243-118-166.us-west-2.compute.amazonaws.com
   ```

   Replace `t2microKey.pem` with the path to your downloaded key file and `ec2-44-243-118-166.us-west-2.compute.amazonaws.com` is the server's public IPv4 domain name. `ubuntu` is the default username for the server.

### Security Note
Ensure that your `.pem` file has the correct permissions. You can set the appropriate permissions using the following command:

```bash
chmod 400 t2microKey.pem
```

## Installing required tools

Now that we have successfully SSH'ed into our remote server we can try out simple commands for example:
```bash title="bash"
$ ll
total 28
drwxr-x--- 4 ubuntu ubuntu 4096 Apr 10 11:47 ./
drwxr-xr-x 3 root   root   4096 Apr 10 11:40 ../
-rw-r--r-- 1 ubuntu ubuntu  220 Mar 31  2024 .bash_logout
-rw-r--r-- 1 ubuntu ubuntu 3771 Mar 31  2024 .bashrc
drwx------ 2 ubuntu ubuntu 4096 Apr 10 11:47 .cache/
-rw-r--r-- 1 ubuntu ubuntu  807 Mar 31  2024 .profile
drwx------ 2 ubuntu ubuntu 4096 Apr 10 11:40 .ssh/
```

We will be presented with the server's clean and default directory structure.

### 1. First we update system packages
```bash title="bash"
sudo apt update
sudo apt upgrade -y
```

### 2. Install Python and pip
Check if Python 3 is already installed. If not, install it along with pip (Python package installer).
Even if Python 3 is installed, running the following scripts will update it to the latest version, so it is a good practice to run these scripts
```bash title="bash"
sudo apt install -y python3 python3-pip
```

### 3. Installing Flask and other necessary libraries
Install Flask, a library to handle geospatial data (like `geopandas` and `fiona`), and any other libraries you might need for KML/Shapefile manipulation. You'll likely need to install the `gdal` and `geos` development libraries first.
```bash title="bash"
sudo apt install -y libgdal-dev libgeos-dev
```
Till now our installation were being done globally. Now to install Flask , geopandas and fiona, we will use pip.
Installing through pip usually gives error if done outside a virtual invironment.
```bash
$ pip3 install Flask geopandas fiona

error: externally-managed-environment



× This environment is externally managed

╰─> To install Python packages system-wide, try apt install

    python3-xyz, where xyz is the package you are trying to

    install.

    

    If you wish to install a non-Debian-packaged Python package,

    create a virtual environment using python3 -m venv path/to/venv.

    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make

    sure you have python3-full installed.

    

    If you wish to install a non-Debian packaged Python application,

    it may be easiest to use pipx install xyz, which will manage a

    virtual environment for you. Make sure you have pipx installed.

    

    See /usr/share/doc/python3.12/README.venv for more information.



note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
```
To solve this problem we will install `python3-venv` and create a `projects` directory inside our server and to work with.
```bash title="bash"
sudo apt install -y python3-venv
mkdir projects
cd projects
```
Now we are ready to create a python virtual environment inside our projects directory and activate it
```bash title="bash"
python3 -m venv venv
source venv/bin/activate
```
By now our `projects` directory structure should look like this:

```
projects/
├── venv/
│   ├── bin/
│   ├── include/
│   ├── lib/
│   └── pyvenv.cfg
```
Install Flask, geopandas, and fiona within the virtual environment: Now that the virtual environment is active, `pip3` will install packages within this isolated environment.
While inside projects directory run :
```bash title="bash"
pip3 install Flask geopandas fiona
```
## Coding API Logic
### 1. Creating a barebones app.py file
Now that we have our libraries installed, we can proceed towards coding API logic.
While still being in python virtual environment, create a directory `simply-flask` inside projects directory
```bash title="bash"
mkdir simply-flask
cd simply-flask
```
Create `app.py` file and paste the following python code into that file. You can use nano editor to do this.
```bash title="bash"
touch app.py
nano app.py
```
Paste following python code 
```py title="python"
from flask import Flask, request, jsonify
import geopandas as gpd
import io
import os

app = Flask(__name__)

# Function to convert point KML to line KML
def convert_point_kml_to_line_kml(kml_string):
    try:
        gdf = gpd.read_file(io.StringIO(kml_string), driver='KML')
        if gdf.geometry.geom_type.iloc[0] == 'Point':
            # Assuming all points belong to a single line
            if not gdf.empty:
                line_geometry = gdf.unary_union.convex_hull.boundary
                line_gdf = gpd.GeoDataFrame([1], geometry=[line_geometry], crs=gdf.crs)
                output_kml = line_gdf.to_kml(index=False)
                return output_kml
            else:
                return "Error: Input KML contains no point geometries."
        else:
            return "Error: Input KML does not contain point geometries."
    except Exception as e:
        return f"Error processing KML: {str(e)}"

@app.route('/convert/point_to_line_kml', methods=['POST'])
def point_to_line_kml():
    if 'kml_file' not in request.files:
        return jsonify({'error': 'No KML file part'}), 400
    file = request.files['kml_file']
    if file.filename == '':
        return jsonify({'error': 'No selected KML file'}), 400
    if file:
        kml_content = file.read().decode('utf-8')
        output = convert_point_kml_to_line_kml(kml_content)
        if "Error" in output:
            return jsonify({'error': output}), 400
        else:
            return output, 200, {'Content-Type': 'application/vnd.google-earth.kml+xml'}

if __name__ == '__main__':
    app.run(debug=False, host='0.0.0.0', port=5000)
```
### 2. Running the flask application
Run the Flask development server: Navigate to the directory containing your app.py file and run:
```bash title="bash"
python3 app.py
```
You should see output similar to
```bash
$ python3 app.py
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://172.31.29.63:5000
Press CTRL+C to quit
```
### 3. Access the API 
You can now send POST requests to your API endpoint. For the point_to_line_kml endpoint, you would send a POST request to http://your_instance_public_ip:5000/convert/point_to_line_kml with the KML file attached in the kml_file field of the form data.

You can use tools like `curl`:
```bash title="bash"
curl -X POST -F "kml_file=@your_point_file.kml" http://your_instance_public_ip:5000/convert/point_to_line_kml -o output.kml
```

(Replace `your_instance_public_ip` and `your_point_file.kml` with your actual values.)
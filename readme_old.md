### Reproducibility with R

## Using Docker and Singularity to run RStudio
 
We are using Docker images that use Rocker images as the base. To build the image on my laptop, I ran:
```
docker buildx build --platform linux/amd64,linux/arm64 --push -t compbiocore/reproducibility_r:202309012 .
```

To run it locally with Docker, you need to install Docker and then run:
```
docker run --rm -p 8787:8787 -e USER=rstudio -e PASSWORD=yourpassword --volume ${PWD}:/home/rstudio compbiocore/reproducibility_r:202309012
```

To run it on Oscar, it is best to use Google Chrome. Then connect to OOD (ood.ccv.brown.edu) and spin up OOD desktop (under 'Default GUIs', then 4 Cores, 15GB Memory, 4 days and 'Launch'), 'Launch Desktop' once running. Then go to the to left corner and click 'Applicatons' and 'Terminal Emulator'.     

Then run:

```
cd ~
mkdir workshops
cd workshops
mkdir -p run var-lib-rstudio-server
printf 'provider=sqlite\ndirectory=/var/lib/rstudio-server\n' > database.conf
```
This will make a folder called `workshops` in you home directory. The rest of these commands come from the Rocker docs on how to use Singularity with Rocker images (https://rocker-project.org/use/singularity.html).   

Then run:
```
cd ~/workshops
export SINGULARITY_BINDPATH="/gpfs/home/$USER,/gpfs/scratch/$USER"
export PASSWORD="mypasswordhere"
```
This changes your directory to the workshops folder and sets some environmental parameters for Singularity. You should change the Password to something other than "mypasswordhere" (I like the xkcd password generator -- https://xkpasswd.net/s/). You might see some message about 'Apptainer', which is fine. Apptainer is just the Singularity installation on Oscar.

Then while still in the `~/workshops` folder, run:

```
cp /gpfs/data/shared/databases/workshops/reproducibility_r/metadata/reproducibility-r-202309012.sif .
```

If you type `ls`, you should see that you have a copy of `reproducibility-r-202309012.sif` in your `~/workshops` folder.

We can run RStudio from the container like this:

```
singularity exec --bind run:/run,var-lib-rstudio-server:/var/lib/rstudio-server,database.conf:/etc/rstudio/database.conf reproducibility-r-202309012.sif /usr/lib/rstudio-server/bin/rserver --auth-none=0 --auth-pam-helper-path=pam-helper --server-user=$(whoami)
```
This will start the container. The `--bind` commands tell singularity where to bind folders in the container -- for example, `singularity exec --bind /data:/mnt my_container.sif` will bind `/data` on the host to `/mnt` in the container. The ` --auth-none=0 --auth-pam-helper-path=pam-helper --server-user=$(whoami)` are lifted directly from the Rocker docs and let us authenticate when we connect to the RStudio container.

Then, open up a new tab in terminal (under 'File') and run:
```
module load firefox/87.0
firefox
```
Point your browser to `localhost:8787`, enter your local user ID (e.g., oscar username) on the system as the username, and the custom password specified in the PASSWORD environment variable.

Then, go to 'File' and 'New Project...'
Pick Create Project from Version Control

You want to use Git, then you'll be prompted for which repo you want to use.
The Repo URL should be: `https://github.com/brown-ccv/reproducibility_r`
Project Directory Name should be: `reproducibility_r`
Create project as a subdirectory of `~/workshops`

Then click 'Create Project'. You'll be prompted for your username and a personal access token. You can make the personal access token on github.com if you click on your profile picture in the top right corner and scroll down to 'Settings'. Look on the left side of the screen and scroll down until you see 'Developer Settings' then click 'Personal Access Token'. On the left of the screen again click 'Personal Access Tokens' and Tokens(classic). Then generate new token (classic) and make a new token with the `repo` box checked. Giving them an expiration date and making a new token every once in a while is not a terrible idea. Copy this token and paste it as the password in rstudio (ctrl + v should work on a mac). Your tokens are like passwords -- keep them secret, keep them safe. 

Once you are in RStudio and ready to run the notebook, look in the bottom right corner. There should be a tab called 'Files'. Look in that tab and open up the notebooks folder, then the quarto folder. Open up 'book.qmd'.

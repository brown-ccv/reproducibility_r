### Reproducibility with R

To build the image on my laptop:
```
docker buildx build --platform linux/amd64,linux/arm64 --push -t compbiocore/reproducibility_r:20230914-3-gcm .
```


To run it locally w docker
```
docker run --rm -p 8787:8787 -e USER=rstudio -e PASSWORD=yourpassword --volume ${PWD}:/home/rstudio compbiocore/reproducibility_r:20230914-3-gcm
```

To run it on oscar, connect to OOD and open up a terminal by going to the blue menu bar at the top of the screen and clicking on 'Clusters' and '>_OSCAR Shell Access'

```
cd ~
mkdir workshops
cd workshops
mkdir -p run var-lib-rstudio-server
printf 'provider=sqlite\ndirectory=/var/lib/rstudio-server\n' > database.conf
```
Best to use Google Chrome for this next bit --  spin up OOD desktop (under 'Default GUIs', then 4 Cores, 15GB Memory, 4 days and "Launch"), 'Launch Desktop' once running. Then go to the to left corner and click 'Applicatons' and 'Terminal Emulator'
Then:
```
cd ~/workshops
export SINGULARITY_BINDPATH="/gpfs/home/$USER,/gpfs/scratch/$USER"
export PASSWORD="mypasswordhere"
singularity exec --bind run:/run,var-lib-rstudio-server:/var/lib/rstudio-server,database.conf:/etc/rstudio/database.conf /gpfs/data/shared/databases/workshops/reproducibility_r/metadata/reproducibility-r-20230914-3-gcm.sif /usr/lib/rstudio-server/bin/rserver --auth-none=0 --auth-pam-helper-path=pam-helper --server-user=$(whoami)
```

then, open up a new tab in terminal (under 'File') and run:
```
module load firefox/87.0
firefox
```
Point your browser to localhost:8787, enter your local user ID (e.g., oscar username) on the system as the username, and the custom password specified in the PASSWORD environment variable.

Then, go to 'File' and 'New Project...'
Pick Create Project from Version Control

You want to use Git, then you'll be prompted for which repo you want to use.
The Repo URL should be: https://github.com/brown-ccv/reproducibility_r
Project Directory Name should be: reproducibility_r
Create project as a subdirectory of ~/workshops

Then click 'Create Project'. You'll be prompted for your username and a personal access token. You can make the personal access token on github.com if you click on your profile picture in the top right corner and scroll down to 'Settings'. Look on the left side of the screen and scroll down until you see 'Developer Settings' then click 'Personal Access Token'. On the left of the screen again click 'Personal Access Tokens' and Tokens(classic). Then generate new token (classic) and make a new token with the `repo` box checked. Copy this token and paste it as the password in rstudio (ctrl + v should work on a mac)

To run the notebook, look in the bottom right corner. There should be a tab called 'Files'. Look in that tab and open up the notebooks folder, then the quarto folder. Open up 'book.qmd'.
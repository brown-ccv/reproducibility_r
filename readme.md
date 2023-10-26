# Reproducible Analysis in R
If you have questions about anything we talk about here, come to our office hours (https://events.brown.edu/ccv/). In addition to the general Oscar office hours, you are also welcome to come by the Computational Biology Core office hours. Even if you aren't a biologist, we can probably help you with your R code.

# Launching RStudio on Singularity with OOD
If you want to try to use the container I used to make this workshop, you can do that using Open On Demand:      

1. Go to [ood.ccv.brown.edu](ood.ccv.brown.edu) and log in with your credentials.
2. On the landing page select the **RStudio on Singularity** app. 



![Landing Page](./notebooks/images/mages/ood_apps_rstudio-sing_highlight.png)

3. Fill in the following fields and leave the other fields empty
   - Number of Hours: 48
   - Num Cores: 4
   - Memory: 16
   - Singularity Container Path: `/gpfs/data/shared/databases/workshops/reproducibility_r/reproducibility_r.sif`
   - Path for R Executable: `~/bin/R`
  
It should look like the two images below.

![OOD Setup 1](./notebooks/images/mages/rstudio_singularity_1.png)

![OOD Setup 2](./notebooks/images/mages/rstudio_singularity_2.png)

If it seems like it is taking too long, you can change `Number of Hours` to `1` for the purposes of following along with this workshop.      

4. Select Launch. The session will now be queued. You will be brought to your interactive sessions dashboard. 

5. When the session is ready a **Connect to RStudio Server** button will appear (see image below). Click this button to launch your instance.

![Ready](./notebooks/images/mages/my_interactive_session.png)

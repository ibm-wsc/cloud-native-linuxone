# It's Time to Get your Pet Clinic Ready for its Internal Debut

In this section, you will bring PetClinic from development to staging for the internal showcase of your pet clinic (staging).

1. [Promotion Tasks](promote.md)
    - Check successful connection of PetClinic `dev` version
    - Deploy PetClinic `staging` version
    - Check successful connection of PetClinic `staging` version
    
2. [Git Tasks](git.md)
    - Add Gogs trigger to pipeline
    - Pass git commit messages and hashes to pipeline
    - Version images by git commit
    - Add webhook to Gogs so it will trigger a new `PipelineRun` for each push

3. [Running Pipeline](action.md)
    - Update PetClinic with new animal type and push to Gogs
    - Watch Gogs trigger `PipelineRun`
    - Watch app move from dev to staging seamlessly with images tagged with git commit SHA 
    - Interact with the PetClinic application in staging using the new animal type
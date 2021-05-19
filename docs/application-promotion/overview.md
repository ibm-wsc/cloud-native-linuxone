# It's time to get your pet clinic ready for its internal debut

In this section, you will bring PetClinic from development to staging for the internal showcase of your pet clinic (staging).

1. [Promotion Tasks](promote.md)
    - Check successful connection of PetClinic `dev` version
    - Deploy PetClinic `staging` version
    - Check successful connection of PetClinic `staging` version
    
2. [Git Tasks](git.md)
    - Add GitHub trigger to pipeline
    - Pass git commit messages and hashes to pipeline
    - Version images by git commit
    - Add webhook to GitHub so it will trigger a new `PipelineRun` for each push

3. [Running Pipeline](action.md)
    - Update PetClinic with new animal type and push to GitHub
    - Watch GitHub trigger `PipelineRun`
    - Watch app move from dev to staging seamlessly with images tagged with git commit SHA 
    - Interact with the PetClinic application in staging using the new animal type

# It's time to get your pet clinic ready for its internal dayview

In this section, you will bring PetClinic from development to staging for the internal showcase opening of your pet clinic (staging).

1. [Promotion Tasks](promote.md)
    - Check rollout of PetClinic `dev` version
    - Deploy PetClinic `staging` version
    - Check rollout of PetClinic `staging` version
    
2. [Git Tasks](git.md)
    - Add GitHub trigger to pipeline
    - Pass Git commit messages and hashes to pipeline
    - Version images by git commit
    - Add webhook to GitHub so it will trigger a new `PipelineRun` for each push

3. [Running Pipeline](action.md)
    - Update PetClinic with new animal and push to GitHub
    - Watch GitHub trigger `PipelineRun`
    - Watch app move from staging to dev seamlessly with images tagged with git commit SHA 
    - Discuss ways to integrate OpenShift on LinuxONE as part of a greater multi-cloud ecosystem

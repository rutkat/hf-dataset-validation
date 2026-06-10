Agent Skill for Huggingface Dataset validation
*How does it work?*

1. Discover the skill in the local skills directory
2. Match the task to the skill’s description
3. Load the SKILL.md instructions into context
4. Run helper scripts

**Test Prompts**
Give Claude these prompts:  
-Prompt 1: "Validate my dataset file @test_data/sample.csv before training."  
-Prompt 2: "Check if the csv file is ready to share."  


```
hf-dataset-validation/
├── SKILL.md             # Main skill file (required)
├── requirements.txt     
├── .gitignore           
├── scripts/             
│   ├── validate_dataset.py
│   └── generate_report.py
├── references/          
│   └── examples.md
└── assets/              
    └── validation-template.txt
```


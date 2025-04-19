# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Alec Khachatryan

```
Existing code (non-reproducible)

1. Target population - Create DataFrame for people at events with initial infection and traced status 
The target population ('ppl', N = 1000) includes attendees at two weddings (2 x 100 = 100 people) and 80 brunches (80 x 10 = 800 people). It is assumed that they haven't been infected or traced prior to attending the events ('infected' = False, 'traced' = True/False). 
2. Sampling frame - Infect a random subset of people (simple random sampling without replacement) 
The sampled frame ('infected_indices', n = 100) represents infected individuals and equals 10% of the target population (attack_rate = 0.10, 'infected' = True). It is generated randomly from the sample frame. This places the target population in two non-overlapping strata ('infected' = False and 'infected' = True). Each individual has equal chances of being selected. 
3. Primary contact tracing (stratified random sampling) 
This sample ('traced', n = 20) is taken from the 'infected' strata and represents randomly tested individuals according to the article that states that an infection has a 20% chance of being traced (trace_success = 0.20). New numbers are generated at each generation of 'infected_indices'. Each individual has equal chances of being selected.  
4. Secondary contact tracing based on wedding / brunch attendance (non-probability sampling). This (event_trace_counts) represents tested individuals according to the article that states that 100% of attendees are tested at those events where at least 2 infections were discovered during primary contact tracing. New numbers are generated at each generation of 'infected_indices'. This is a non-probability sample as all those who have attended specific events will be tested. 
5. Calculate proportions of infections and traces
These proportions are equal and new values are generated at each generation of 'infected_indices'. The proportions are calculated according to the basis probability formula:  P(event) = (favorable outcomes) / (total outcomes). Similarly, when the simulation is run many times, in the resulting data frame (props_df) 'infections' equal 'traces' indicating that all infections are traced. These proportions are also seen on the resulting histogram. 

Amended code (reproducible) 

# Setting seed for reproducibility
np.random.seed(1)
Setting seed ensures that the results of our experiments can be reproduced. I set seed with a value of 1, but I can use any seed value. As long as the seed value remains the same, the random numbers generated in the model will be the same, meaning that the resulting distributions, proportions, and histograms will be similar.  


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 09/04/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.

## 📅 Weekly Notes

  

### Week 2: CLI Intro/work

📌 **Important Takeaways:**

- I never knew cd would take you back to your home folder, nugget of gold...
- I learned some far more effecient ways of removing and dealing with folders while working inside them, etc.
- I finally had a valid reason to spend time learning about Google cloud and spinning up a VM in the GCLOUD!
---

  

### Week 3: Nano, Git, GitHub, Markdown

📌 **Important Takeaways:**

- Spent hours finally focusing on using MARKDOWN, which I used to hate. It's really like VI, once you learn it, it's quick and not scary at all! This repo is an example!

- Git aliases are helpful, I was always too lazy to create any bash/zsh aliases for GIT but now I've done so, and it's helpful. 
---

### Week 4: GREP (searching)

📌 **Important Takeaways:**

- cat file and pipe into grep, using it can reduce regex needs. For example, grep -i "first" records.marc will find first or even words like "firsthand", etc. regardless of case. -v swapped the search to an inverse, nice.
- scopus, never used it previously, excellent resource from UK library
---

### Week 5: APT and YAZ for the win...

📌 **Important Takeaways:**

- fairly well versed with apt and it's functions/use.
- YAZ, never worked with it but did learn that UK has a connection server: saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY
- yaz-client then open to the server then you can use find commands mixed with the syntax of searching by 1=4 (title), etc.
- yaz does JSON. using the -m we can capture our search results in a marc file then use yaz-marcdump -o json marcfile > jsonfile
- yaz-marcdump does xml as well (-o marcxml)
- if you want to get all results to the file for processing later, you can do (show 1 +XXX) to grab them all
- library of congress server: z3950.loc.gov:7090/voyager


### LAMP Stack!

📌 **Important Takeaways:**

- w3m/elinks is a text browser that I can use from CLI, may be handy versus curl/wget in some cases
- web url: http://34.148.65.51/
- command to create a user in cli for mysql: create user 'opacuser'@'localhost' identified by 'abc123!a88Da';
- export MYSQL_PS1="[\d]> " will give us a nicer mysql CLI prmopt that shows the active db selected by "use"

---

## 📝 Final Thoughts & Reflections

TBD, check back near the end of the semester! ;)

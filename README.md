<div align="center">

# Steam Market Evaluation

</div>

---

### **Scenario and Objective:**

Gillian Games (Not a real company) is a new indie games studio looking to publish videogames. They only recently acquired the necessary production funding, so success with the first few games released is paramount. In order to ensure their success, the company has asked me and a few other analysts to take some data from Steam and find the standard trends of the industry. The dataset spans from 2017-2024, and it contains several videogame titles and their subsequent attributes and financial performance. I will plug this dataset into Power BI in order to answer the following questions:

- Do more games have Mac interface, Linux interface or both?
- Do games usually have a required age? If so, what is the distribution?
- What are the most common tags and genres for videogames on Steam?
- What common languages are usually available for a game?

Gillian Games has also instructed me to find out the general financial trends of the industry. To accomplish this, I will answer the following questions:

- Which genres are associated with the most revenue and the least revenue?
- How have revenue, total players, total games and average price changed from 2017-2024?

### **Data Report:**

<img width="1192" height="668" alt="image" src="https://github.com/user-attachments/assets/b2c26dcc-4870-4818-9c01-fd1a69e5dce3" />  
  
### **Data Preprocessing:**

The challenge of this project was that it was done completely in Power BI. There was no Python preprocessing or SQL analysis. In Power BI, most cleaning can be done in the Power Query, which operates similarly to Excel. Because of this, most general preprocessing was smooth, but when it came to construction of the data model itself, there was one major hurdle to solve: The bridge tables.

One of the most useful indicators in this dataset are Genres and Tags, as they indicate the kinds of games that are popular. However, their data structure was difficult to work with, since each videogame contains multiple genres and tags. These genres and tags were all placed into 1 cell per row and delimited by a comma like so:

<img width="1117" height="161" alt="image" src="https://github.com/user-attachments/assets/b04e1464-e4ec-48cf-b885-2eb688c97ae3" />
<br>

In Python, I would simply explode the data in a seperate table, but since everything is done in Power BI, it must be done in Power Query (Basically Excel). Thankfully, Power Query has a mechanism for splitting rows, so the process was simple. For genres, I created a seperate 'genre' table that contained the AppID (the dataset's primary key) and the genres in their raw form, as seen above. Then I created a bridge table 'genre bridge' that splits the data into one genre per row and duplicates the App ID. as shown below:

<div align="center">
<img width="398" height="293" alt="image" src="https://github.com/user-attachments/assets/17949c5d-e64a-4049-aca3-f2caf96c6a7c" />
</div>
<br>

This ensures each individual tag flows through the model as a many-to-1 relationship. I repeated the process for 'Tags' and 'Language.' Below is the final model:
<br>

<img width="1422" height="713" alt="image" src="https://github.com/user-attachments/assets/21f080cf-6cc5-4693-9e6c-5cfbddb41745" />

It looks complex, but the important thing is that everything in the model has a 1-to-many, many-to-1 or 1-to-1 relationship, ensuring smooth filtration.

### **Analysis and Results:**

Let's looks at some of the visuals and answer the previous data questions.

- Do more games have Mac interface, Linux interface or both?
- Do games usually have a required age? If so, what is the distribution?

<img width="1072" height="252" alt="image" src="https://github.com/user-attachments/assets/212be289-6795-496f-af20-e8b883b749b2" />

Based on these visuals, A vast majority of games have **English** as a language, support **Mac or Linux + Mac** (Linux only seems like a disasterous idea). Most games also have no required age, allowing them to cast a wide audience net outside of specific niches.

- What are the most common tags and genres for videogames on Steam?

<img width="1156" height="297" alt="image" src="https://github.com/user-attachments/assets/9855ff21-31cb-4a3d-9029-f97957c26d75" />

- What are some of the revenue statistics of Steam Games?

<img width="1127" height="212" alt="image" src="https://github.com/user-attachments/assets/885c31d4-dc13-4af4-a473-9c72d0acb347" />

Based on these visuals, it appears that player count follows revenue very closely. This is supported by the fact that the overall average price of videogames doesn't fluctuate very much. The only exception to this matching trend is 2023.

There isn't a strong relationship between number of games released and their average price. More and more games have been released each year without fail, but average price remains between 6-7 dollars.

Action, Adventure and RPG games have high revenue compared to the number of games. Indie and Casual games are the opposite, they lack revenue results despite high distributions. For Indie games, the trend isn't necessarily negative. Indie studios are numerous and Indie games are the most dominant overall genre, and have a lower ceiling for creation, leading to more dubious financial results. Casual games have no such excuse, and might not be a good fit for these initial releases.

### **Analyst Comments:**

- When looking over these numbers, it's important to note that 'Genre' and 'Tag' statistics regarding revenue should be taken with a grain of salt, as there are more genre descriptions than games themselves multipled by the raw videogame revenue. The most reliable statistic is the count of genres.

- Since Gillian Games is looking to cast a wide net for the highest likelihood of success, I have found and targeted the highest or majority distributions. For more niche analyses regarding specific genres, further specification and analysis of data is needed.

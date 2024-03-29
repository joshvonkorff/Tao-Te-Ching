# Tao Te Ching
Using Retrieval Augmented Generation with OpenAI and Pinecone

In this project, I have used Pinecone and Retrieval Augmented Generation to query the Tao Te Ching in the context of a user-provided question.  For example, if the user asked "Do students learn better by reading books or by performing homework exercises?", the notebook would first store the Tao Te Ching as 81 separate documents in Pinecone, which is a popular vector database. (The Tao Te Ching is naturally divided into 81 sections.)  The notebook will then find the document (out of the 81) that is most similar to the user's question, and use it to answer the question.

Pinecone stores the 81 documents and assigns an embedding vector to each - a vector that summarizes the meaning of each document as a set of about 1,500 numbers.  The idea is that if two documents have similar meaning, their vectors will be similar to one another.  Thus, "I played with my dog" and "I like being around my cat" would have very similar vectors because they are both about enjoying the company of pets.  In contrast, "I played with my dog" and "carrots are a vegetable" would have very different vectors.

Thus, if the user asks "Are bats a kind of bug?", we might assign the vector (1.2, 0, -0.8).  (Pretending for a moment that 3 numbers are enough, when really it's more like 1,500.)  Then Pinecone finds the most similar document to help answer the question:

"Bats are not a kind of bug." (1.1, 0.1, -0.5)  
"Bugs bunny is a kind of rabbit." (0.9, 0.8, -0.2)  
"A baseball bat is made of wood." (-0.2, -1, -0.9)  

In this toy example, the first is the most similar vector, so it is going to be used to answer the question.

You will need to provide your own Pinecone API key as well as an OpenAI API key to make this work.  I assume a free starter environment in Pinecone, which allows for a single index only; but you can easily change to a bigger environment.  (A Pinecone index is like a table in SQL.)

When the vector database stores embedding vectors for different objects, such as blocks of text, we can locate the most relevant block of text to any query, e.g. using cosine similarity.  Cosine similarity means that two vectors (1.2, 0, -0.8) and (1.1, 0.1, -0.5) would have their similarity calculated something like: 1.2 * 1.1 + 0 * 0.1 + (-0.8) * (-0.5).  (Only, with normalization.)

The translation used for the Tao Te Ching belongs to Wikisources:

https://en.wikisource.org/wiki/Translation:Tao_Te_Ching

Then, the notebook uses OpenAI to answer the user's question, selecting the most relevant Tao Te Ching passage as context.  In this case, passage #27 was selected.

The user input the above question (which is not visible in the notebook) as to whether students learn better using book or homework exercises.

Note that this question and answer were used in the past.  They may change from time to time as I work with the code; the current version of the notebook may contain a different question and answer.

At the time I asked the above question, the notebook gave the following answer:

> According to the quote from the Tao Te Ching, students may learn better by performing homework exercises rather than just reading books. The quote emphasizes the importance of practice and taking action. It suggests that through practice (in this case, doing homework exercises), there is always something to gain, implying that active engagement with the material leads to growth and learning. On the other hand, the quote implies that those who are preoccupied (potentially with just reading books) cannot master the universe (representing knowledge or understanding).
> In applying this to the question at hand, students may benefit more from performing homework exercises rather than solely relying on reading books. While reading books can provide valuable information, it is through practical application, such as doing exercises and actively engaging with the content, that students can truly deepen their understanding and consolidate their knowledge. By taking action and putting the learned concepts into practice, students are more likely to retain the information, develop critical thinking skills, and gain a better understanding of how to apply the knowledge in real-world situations.

The passage from Wikisource's translationw was as follows:

> 49. Practicing scholarships, everyday there is something to gain,
> Practicing the Dao, everyday there is something to lose;
> When you lose all that can be lost,
> You may be without action.
> Act not and leave none to be acted upon!
> Therefore those who can master the universe,
> Often remain unoccupied;
> Those who are preoccupied,
> Cannot master the universe.

The approach for querying Pinecone is based on the instructions in Sinan Ozdemir's "Quick Start Guide to Large Language Models: Strategies and Best Practices for Using ChatGPT and Other LLMs"  (Chapter 2).

In chapter 3, Ozdemir suggests RAG, though not by that name.  An example paper about RAG is: https://proceedings.neurips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html

Wikisource is scraped from https://en.wikisource.org/wiki/Translation:Tao_Te_Ching using Beautiful Soup.

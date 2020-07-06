## Author Aggregation

- How to deal with author/affiliation to country attribution? How to weigh since one paper should only sum up to 1 total count. Three alternatives:
- Considering just the first author.
- Considering last author
- Aggregation of Authors and assigning weights according to the number of authors from a country. Ex: If for a particular paper there are 2 authors from USA and 1 from UK then it will assign 0.67 to USA and 0.33 to UK.
<a href="https://drive.google.com/file/d/1HBr2BQFt_e_gvmPqRZlIuHOjj9ZM67A4/view?usp=sharing">Example</a>.
- Ex: Consider <a href="https://arxiv.org/pdf/1412.6980v8.pdf">this</a> paper. It has two authors one from Netherlands and another from Canada so in this case we have three alternatives:
1. Considering only 1st author: Netherlands = 1,Canada = 0
2. Considering only last author: Netherlands = 0,Canada = 1
3. Aggregation of author and assigning weights: Netherlands = 0.5,Canada = 0.5

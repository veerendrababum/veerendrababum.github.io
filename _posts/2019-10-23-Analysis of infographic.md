---
title: "Analysis of an Info-graphic using Alberto Cairo's principles"
date: 2019-10-22
tags: [Data Science Projects]
excerpt: "Data Analysis, Data Visualization, Data Science"
---
<p style="text-align: justify;font-family: none;">Alberto Cairo’s work entitled “Reflections on the Challenges and Pitfalls of Evidence-Driven Visual Communication” presents three ways graphics can mislead the reader:</p>
<p style="text-align: justify;font-family: none;">•	Hiding relevant data to highlight what benefits us: “cherry-picked” statistics, e.g. show the average to hide that sales were negative in a certain moment.</p>
<p style="text-align: justify;font-family: none;">•	Displaying too much data to obscure reality: extreme and unnecessary detail, e.g. showing details that would be better explored with interactive tools, not on a printed page.</p>
<p style="text-align: justify;font-family: none;">•	Using graphic forms in inappropriate ways: distorting the data, e.g. truncate Y-axis, hide where X-axis begins, inconsistent intervals in one of the axis, etc.</p>
<p style="text-align: justify;font-family: none;">I’m using these guidelines to analyze and detect issues in my own visual graphics. With examples, the author shows that these mistakes do mislead audiences, and creators of data visuals should aim to eliminate or, at least, minimize them.</p>
<p style="text-align: justify;font-family: none;">Inspired by this work, I selected an infographic, published at <a href="https://slge.org/resources/infographic-state-and-local-government-compensation">Center for State and Local Government Excellence</a>, to analyze it according to Cairo’s mechanisms. This infographic shows visualizations of trends in state and local government employee compensation from 2006 to 2016. Areas of focus include total compensation, compensation breakouts by component, and comparisons to the private sector. Key findings include:</p>
<p style="text-align: justify;font-family: none;">•	Wages and salaries as a portion of total compensation has been declining (from 67 to 63 percent), as health insurance and defined benefit retirement costs have risen. A fourth component of total compensation — other benefits — has stayed relatively unchanged at 15 to 16 percent.</p>
<p style="text-align: justify;font-family: none;">•	While there has been volatility in individual components of compensation, total compensation has stayed fairly consistent, averaging about a 2 percent increase in employer costs per year.</p>
<p style="text-align: justify;font-family: none;">•	The costs of both health benefit and defined benefit retirement have increased significantly since 2006.</p>
<img src="{{site.url}}{{site.baseurl}}/images/InfoGraphic.jpg" alt=""> 
<p style="text-align: justify;font-family: none;">The components that I’ve found misleading in the visual are organized by the mechanisms presented by Alberto Cairo’s work:</p>
### Hiding Relevant Data to Highlight What Benefits Us:
<p style="text-align: justify;font-family: none;">The visual doesn’t show the data on employees’ pay rate based on experience/age. For an example, consider if a number of highly experience people retire and the government hires a large number of low-experience people with low wages. In this scenario, the average salary rate should be effected with this factor and affect the compensation results between 2006 to 2016. If the visual also showed a bar graph for the employees wage rate based on experience/age and number of employees working in each age group/experience range, the decoder could better compare these consumptions.</p>
### Displaying Too Much Data to Obscure Reality:
<p style="text-align: justify;font-family: none;">There are eight data visuals presented</p>
<p style="text-align: justify;font-family: none;">1.	Share Of Total Compensation<br />
2.	Total Compensation<br />
3.	Compensation Breakouts<br />
4.	Total Compensation<br />
5.	Percentage With At Least A Bachelor’s Degree<br />
6.	Total Expenditures Per Hour Worked<br />
7.	Comparisons To Private Sector<br />
8.	Compensation And College Education<br />
</p>
<p style="text-align: justify;font-family: none;">Showing these eight data visuals in one infographic is misleading because it can confuse the decoder with unclear links between them. What is the relationship between Compensation And College Education and Share Of Total Compensation? Is the data from the same source? In the Compensation And College Education, there is an element for “Private Sector” but there is no similar figure available in the chart Share Of Total Compensation.</p>
### Using graphic forms in inappropriate ways (Distorting the Data):
<p style="text-align: justify;font-family: none;">For instance, Share Of Total Compensation and Total Compensation graphs are related to each other and sharing the same information. However, the bar color for “Health Insurance” is  colored blue in Share Of Total Compensation and but its yellow in Total Compensation. It would be made clear to the decoder by choosing the same bar color to represent the same element in both Share Of Total Compensation and Total Compensation graph.</p>
### Conclusion
<p style="text-align: justify;font-family: none;">From my experience, I am supportive of the possibility that photos merit a thousand words. Embellishments have been valuable in my day by day work with business procedures and insights diagrams with incredible outcomes.Connecting back to Cairo's work, my perspective is that the advantages of embellishments will be more powerful if they are accurate to inform their audiences.</p>
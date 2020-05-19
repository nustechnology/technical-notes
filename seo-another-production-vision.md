As a standard software development I knew, for a successful release, a website must pass some non-functional requirements
+ Performance: The website must be fast. Refer some statistic report, 70% user will leave the website with more than 2 second loading time
+ Security: This is an essential non-functional requirement, a less-secure website cannot keep the users there
+ Scalability: Website should be able to scaling. Day to day, the amount of website traffic will increase, and the current configuration cannot be satisfied
+ Maintainability: Website should be easy for updating. After going live, it still can be updated for various reason
<br/>
...

However, recently, I realized another important non-functional requirement, it should be done as the beginning requirement for most of website. This is SEO


#### What is SEO?
SEO stands for Search Engine Optimization, which is the practice of increasing the quantity and quality of traffic to your website through organic search engine results.

Without SEO, users are hard to know the website, so they will be hard to access it. So this is really the start point

In fact, we are not SEO expert, so we cannot make a website lie on the top ten Google search results. However, we still need to know about SEO concepts as much as possible and apply some simple practices. At least, we will reduce the SEO cost for website when an SEO expert review it 

I'm also not an SEO expert, here are all my own experience I collected when customer ask me to make to improve SEO score from SEO expert advisors

#### Actions

Firstly, we need to know how search engines (example Google) work. This contains two steps

+ Crawling: Google will visit website to read information
+ Indexing: Google will score the website and rank it 

So, you should imagine how Google read information from website and supply it as most correct as possible data. 

1. Website domain: 
-  Website domain should be relative with website content. For example, if website is about travel, we should choose an appropriate domain like "tripadvisor", "travel.com" ...

2. Site map should be available
- You can imagine a website look like a book. A book without bookmark is terrible, isn't it? Site map is similar, it explains in short which contained on the website

3. Page URL should be friendly 
- You should avoid using a "magic" number on the link and use a friendly name instead
Ex: 

    ```
    #BAD
    /products/1

    #GOOD
    /product/whatever-product-name
    ```

4. Page should have some appropriate data for Google. 

+ Title and description elements: Please don't forget those elements when building a page. They are invisible with you so maybe you will forget them, but be aware that 
+ Document structure: If you can, please structure your page as structure: One H1 element as page heading, use H2 as subtitle
+ Image SEO optimization: Not like the user, Google cannot see your image, this read image by image name and image alt attribute. So please don't forget name the image correctly and fill the image alt attribute.

    ```
    #BAD
    <img src="image1.png" />

    #GOOD
    <img src="blue_skype.png" alt="blue skype" />
    ```


#### Advance actions
+ Keywords: This is really one of important SEO factor. If you can support keywords of your website for Google. This will be easier to detect you when user search such keyword.

+ Support Rich snippet:  An enhanced result in Google search with extra visual or interactive features, sure that Google will consider the website have Rich snippet is higher than not

+ Frequently perform SEO checker: This can be considered as a testing step as we will check the new page with SEO to make sure this SEO-able
You can use various online SEO checker tools such as semrush or woorank


As I stated above, I'm not SEO expert, so please feel free to feedback for contribution about SEO practice you think that this can be done by the developer to make the website more SEO friendly.



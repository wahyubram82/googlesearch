googlesearch
============

Google search from Python.

https://python-googlesearch.readthedocs.io/en/latest/

Usage example
-------------

    # Get the first 20 hits for: "Breaking Code" WordPress blog
    from googlesearch import search
    for url in search('"Breaking Code" WordPress blog', stop=20):
        print(url)

Another Example with crawling The Label / Titile of URL from google, with time range:
    
    from googlesearch import search
    
    #prepareing module to avoid block by target domain for exceed limit access:
    from googlesearch import get_random_user_agent as useragent
    userid = useragent().decode('utf-8')
    home_folder = os.getenv('HOME')
    cookie_jar = LWPCookieJar(os.path.join(home_folder, '.google-cookie'))
    
    #module to set time
    from googlesearch import get_tbs as getdate
    from datetime import date, timedelta
    
    #module to pure link / url that crawled
    from tldextract import extract
    
    keywords = 'what you want to search'
    site_to_search = 'example.com'
    
    #Get all result in last 7 days
    #all variations of 'time limitation for search', depend on your knowledge about datetime module
    
    for x in range(0,7):
        start_time = date.today() - timedelta(days=(x+1))
        end_time = date.today() - timedelta(days=x)
        timetarget = getdate(start_time, end_time)
        
        #clear cookies to increase possiblity google cannot detect you
        cookie_jar.clear()
        
        for i in search(querry, lang = 'id-ID',tbs=timetarget, num=10, start=0, stop=None, pause=3, user_agent=userid):
            link = i.split(' -- ')[1]
            label = i.split(' -- ')[0]
            a, dom, tld = extract(link)
            pure_source = dom + '.' + tld
            print('url label: ', label)
            print('the url is: ', link)
            print('the pure domain is: ', pure_source)
            
            #only doing this once or twice is not a problem, 
            #but more often you do this, you will got error brcause to many request.
            #to preven it, do another job here (while iteration run), example:
            from newspaper import Article
            article = Article(i)
            #do another command here...
            #works for me while to crawl news paper for full two month  (60 days)
            
            
            
    


Installing
----------

    pip3 install google
    OR
    pip install google

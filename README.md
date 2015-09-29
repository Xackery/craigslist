# craigslist
Simple GoLang Craigslist Crawler

A simple example:

```
package main

import (
	"fmt"
	"time"
	"github.com/xackery/craigslist"
)

func main() {
	client := &craigslist.Client{}
	client.UseStoredOffset = true
	//Change seattle to the subdomain inside craigslist.com, and sof to the suffix,
	// e.g. http://seattle.craigslist.com/search/sof

	searchList, err := client.GetSearchList("seattle", "sof")
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println("Got", len(searchList.Searches), "results")
	//iterate results
	for _, search := range searchList.Searches {
		fmt.Println(search.Url, search.Id, search.Title)
		keywords := []string{"php", "lamp", "golang"}
		keywordResults, err := client.SearchPageForKeywords(search.Url, keywords) //put your terms here
		if err != nil {
			fmt.Println(err.Error())
		}
		fmt.Println("Found", len(keywordResults), "keywords")
		time.Sleep(10 * time.Second) //craigslist doesn't like too many requests.
	}
}

```
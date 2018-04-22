# NLP
# download tweets clean up the corpus
setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)
tw = twitteR::searchTwitter("#metoo", n = 2000, since = '2017-01-01', retryOnRateLimit = 1e3)
d = twitteR::twListToDF(tw)

# insurance
# write.csv(d, "D:/Data 902/Kidambi again/NLP Final Project/tweets.csv")

d = read.csv("D:/Data 902/Kidambi again/NLP Final Project/tweets.csv")

# strip out non alpha numeric characters 
d$text <- iconv(d$text, from = "UTF-8", to = "ASCII", sub = "")

# vector of the corpus  
review_corpus <- VCorpus(VectorSource(d$text))

#Cleaning corpus - pre_processing
clean_corpus <- function(cleaned_corpus){
  removeURL <- content_transformer(function(x) gsub("(f|ht)tp(s?)://\\S+", "", x, perl=T))
  cleaned_corpus <- tm_map(cleaned_corpus, removeURL)
  # cleaned_corpus <- tm_map(cleaned_corpus, content_transformer(replace_abbreviation))
  cleaned_corpus <- tm_map(cleaned_corpus, content_transformer(tolower))
  cleaned_corpus <- tm_map(cleaned_corpus, removePunctuation)
  cleaned_corpus <- tm_map(cleaned_corpus, removeNumbers)
  cleaned_corpus <- tm_map(cleaned_corpus, removeWords, stopwords("english"))
  # available stopwords
  # stopwords::stopwords()
  custom_stop_words <- c("https","metoo", "½í²o")
  # custom_stop_words = iconv(cleaned_corpus, to = "ASCII//TRANSLIT")
  cleaned_corpus <- tm_map(cleaned_corpus, removeWords, custom_stop_words)
  # cleaned_corpus <- tm_map(cleaned_corpus, stemDocument,language = "english")
  cleaned_corpus <- tm_map(cleaned_corpus, stripWhitespace)
  return(cleaned_corpus)
}

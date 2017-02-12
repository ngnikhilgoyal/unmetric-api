require(httr)

## Unmetric API - Instagram Account Endpoint

# Enter current access token
accesstoken <- "xxxxxx"

# enter date range
startdate <- "xxxxxx"
enddate <- "xxxxxx"

# instagram handles to get data for
handle_list <- c('xxxxx', 'yyyyy', 'zzzzz', 'aaaaa', 'bbbbb', 'ccccc')

api_call_reqs <- paste0("https://api.unmetric.com/v2/api/ins/", handle_list, "?accesstoken=", accesstoken, "&startdate=" , startdate, "&enddate=" , enddate)

api_response <- lapply(api_call_reqs, function(x) GET(x))
response_list <- lapply(api_response, function(x) content(x))
response_df <- do.call(dplyr::bind_rows, response_list)

remove(api_response, response_list)


########################## aggregated metrics for account#############################
aggregate_call_reqs <- paste0("https://api.unmetric.com/v2/api/ins/", IG_handles, 
                              "/metrics/aggregate?startdate=" , startdate, "&enddate=" , enddate, "&accesstoken=", accesstoken)

## get api response
aggregate_post_response <- lapply(aggregate_call_reqs, function(x) GET(x))

## extract json into list
aggregate_response_list <- lapply(aggregate_post_response, function(x) content(x))

## convert to data frame
aggregate_response_df <- do.call(dplyr::bind_rows, aggregate_response_list)


############################ POST LEVEL DATA #############################################

## generate URL
post_call_reqs <- paste0("https://api.unmetric.com/v2/api/ins/", IG_handles, 
                              "/posts?accesstoken=" accesstoken, "&startdate=", startdate, "&enddate=" , enddate)
## get api response
post_level_response <- lapply(post_call_reqs, function(x) GET(x))

## extract json
post_response_list <- lapply(post_level_response, function(x) content(x))

## convert to dataframe
# repeat brand names as many times as count of posts
sapply(post_response_list, "[[", 1)
brand_name <- data.frame(brand_name = rep(sapply(post_response_list, "[[", 1), c(sapply(sapply(sapply(post_response_list, "[[", 4), "["), length))))

# unlist post responses and collapse to data frame
mylist <- unlist(sapply(post_response_list, "[[", 4), recursive = FALSE)
temp <- do.call("rbind", mylist)

post_level_data <- cbind(brand_name, as.data.frame(temp[ ,1:8]))
names(post_level_data)[9] <- "unmetric_engagement_score"

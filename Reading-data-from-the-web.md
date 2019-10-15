Reading data from the web
================
Olya Besedina

# get the data

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_xml = read_html(url)

drug_use_xml %>% 
  html_nodes(css = "table")

# now you have all 15 tables, but you want just the first one

drug_use_xml %>% 
  html_nodes(css = "table")%>% 
  .[[1]]

# the same as 
  table_list = drug_use_xml %>% 
 html_nodes(css = "table") 
  table_list = [[1]]
```

.\[\[1\]\] extracts the first table dot before the brackets = everything
that was before the pipe now represented by the dot table\_list is the
same as .

``` r
table_marj = 
  (drug_use_xml %>% html_nodes(css = "table")) %>% 
  .[[1]] %>%
  html_table() %>%
  slice(-1) %>% 
  as_tibble()
```

# extract table kfrom teh web

``` r
nyc_cost = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") %>%
  html_nodes(css = "table") %>%
  .[[1]] %>%
  html_table(header = TRUE)
```

# harry potter data

``` r
hpsaga_html = 
  read_html("https://www.imdb.com/list/ls000630791/")
```

``` r
hp_movie_names =
  hpsaga_html %>% 
    html_nodes(".lister-item-header a") %>%
  html_text()

hp_movies_runtime = 
  hpsaga_html %>% 
  html_nodes(".runtime") %>% 
  html_text()

hp_df = 
  tibble(
    title = hp_movie_names,
    runtime = 
  )
```

# amazon

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

# these are reviews from the first page only

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

# using API

Use GET. content() make s guess about how table is supposed to look like
and make csv file

``` r
nyc_water_df = 
  GET("https://data.cityofnewyork.us/resource/waf7-5gvc.csv") %>% 
  content()
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

# json gives you brackets and stuff. requires additional parsing. So csv is easier to use

``` r
nyc_water_df = 
  GET("https://data.cityofnewyork.us/resource/waf7-5gvc.json") %>% 
  content("text") %>%
  jsonlite::fromJSON() %>%
  as_tibble()
```

``` r
brfss_smart2010 = 
  GET("https://data.cdc.gov/api/views/acme-vg9e/rows.csv?accessType=DOWNLOAD") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   Year = col_double(),
    ##   Sample_Size = col_double(),
    ##   Data_value = col_double(),
    ##   Confidence_limit_Low = col_double(),
    ##   Confidence_limit_High = col_double(),
    ##   Display_order = col_double(),
    ##   LocationID = col_logical()
    ## )

    ## See spec(...) for full column specifications.

``` r
poke = 
  GET("http://pokeapi.co/api/v2/pokemon/1") %>%
  content()

poke
```

    ## $abilities
    ## $abilities[[1]]
    ## $abilities[[1]]$ability
    ## $abilities[[1]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## $abilities[[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
    ## 
    ## 
    ## $abilities[[1]]$is_hidden
    ## [1] TRUE
    ## 
    ## $abilities[[1]]$slot
    ## [1] 3
    ## 
    ## 
    ## $abilities[[2]]
    ## $abilities[[2]]$ability
    ## $abilities[[2]]$ability$name
    ## [1] "overgrow"
    ## 
    ## $abilities[[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## $abilities[[2]]$is_hidden
    ## [1] FALSE
    ## 
    ## $abilities[[2]]$slot
    ## [1] 1
    ## 
    ## 
    ## 
    ## $base_experience
    ## [1] 64
    ## 
    ## $forms
    ## $forms[[1]]
    ## $forms[[1]]$name
    ## [1] "bulbasaur"
    ## 
    ## $forms[[1]]$url
    ## [1] "https://pokeapi.co/api/v2/pokemon-form/1/"
    ## 
    ## 
    ## 
    ## $game_indices
    ## $game_indices[[1]]
    ## $game_indices[[1]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[1]]$version
    ## $game_indices[[1]]$version$name
    ## [1] "white-2"
    ## 
    ## $game_indices[[1]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/22/"
    ## 
    ## 
    ## 
    ## $game_indices[[2]]
    ## $game_indices[[2]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[2]]$version
    ## $game_indices[[2]]$version$name
    ## [1] "black-2"
    ## 
    ## $game_indices[[2]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/21/"
    ## 
    ## 
    ## 
    ## $game_indices[[3]]
    ## $game_indices[[3]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[3]]$version
    ## $game_indices[[3]]$version$name
    ## [1] "white"
    ## 
    ## $game_indices[[3]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/18/"
    ## 
    ## 
    ## 
    ## $game_indices[[4]]
    ## $game_indices[[4]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[4]]$version
    ## $game_indices[[4]]$version$name
    ## [1] "black"
    ## 
    ## $game_indices[[4]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/17/"
    ## 
    ## 
    ## 
    ## $game_indices[[5]]
    ## $game_indices[[5]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[5]]$version
    ## $game_indices[[5]]$version$name
    ## [1] "soulsilver"
    ## 
    ## $game_indices[[5]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/16/"
    ## 
    ## 
    ## 
    ## $game_indices[[6]]
    ## $game_indices[[6]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[6]]$version
    ## $game_indices[[6]]$version$name
    ## [1] "heartgold"
    ## 
    ## $game_indices[[6]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/15/"
    ## 
    ## 
    ## 
    ## $game_indices[[7]]
    ## $game_indices[[7]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[7]]$version
    ## $game_indices[[7]]$version$name
    ## [1] "platinum"
    ## 
    ## $game_indices[[7]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/14/"
    ## 
    ## 
    ## 
    ## $game_indices[[8]]
    ## $game_indices[[8]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[8]]$version
    ## $game_indices[[8]]$version$name
    ## [1] "pearl"
    ## 
    ## $game_indices[[8]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/13/"
    ## 
    ## 
    ## 
    ## $game_indices[[9]]
    ## $game_indices[[9]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[9]]$version
    ## $game_indices[[9]]$version$name
    ## [1] "diamond"
    ## 
    ## $game_indices[[9]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/12/"
    ## 
    ## 
    ## 
    ## $game_indices[[10]]
    ## $game_indices[[10]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[10]]$version
    ## $game_indices[[10]]$version$name
    ## [1] "leafgreen"
    ## 
    ## $game_indices[[10]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/11/"
    ## 
    ## 
    ## 
    ## $game_indices[[11]]
    ## $game_indices[[11]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[11]]$version
    ## $game_indices[[11]]$version$name
    ## [1] "firered"
    ## 
    ## $game_indices[[11]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/10/"
    ## 
    ## 
    ## 
    ## $game_indices[[12]]
    ## $game_indices[[12]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[12]]$version
    ## $game_indices[[12]]$version$name
    ## [1] "emerald"
    ## 
    ## $game_indices[[12]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/9/"
    ## 
    ## 
    ## 
    ## $game_indices[[13]]
    ## $game_indices[[13]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[13]]$version
    ## $game_indices[[13]]$version$name
    ## [1] "sapphire"
    ## 
    ## $game_indices[[13]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/8/"
    ## 
    ## 
    ## 
    ## $game_indices[[14]]
    ## $game_indices[[14]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[14]]$version
    ## $game_indices[[14]]$version$name
    ## [1] "ruby"
    ## 
    ## $game_indices[[14]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/7/"
    ## 
    ## 
    ## 
    ## $game_indices[[15]]
    ## $game_indices[[15]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[15]]$version
    ## $game_indices[[15]]$version$name
    ## [1] "crystal"
    ## 
    ## $game_indices[[15]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/6/"
    ## 
    ## 
    ## 
    ## $game_indices[[16]]
    ## $game_indices[[16]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[16]]$version
    ## $game_indices[[16]]$version$name
    ## [1] "silver"
    ## 
    ## $game_indices[[16]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/5/"
    ## 
    ## 
    ## 
    ## $game_indices[[17]]
    ## $game_indices[[17]]$game_index
    ## [1] 1
    ## 
    ## $game_indices[[17]]$version
    ## $game_indices[[17]]$version$name
    ## [1] "gold"
    ## 
    ## $game_indices[[17]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/4/"
    ## 
    ## 
    ## 
    ## $game_indices[[18]]
    ## $game_indices[[18]]$game_index
    ## [1] 153
    ## 
    ## $game_indices[[18]]$version
    ## $game_indices[[18]]$version$name
    ## [1] "yellow"
    ## 
    ## $game_indices[[18]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/3/"
    ## 
    ## 
    ## 
    ## $game_indices[[19]]
    ## $game_indices[[19]]$game_index
    ## [1] 153
    ## 
    ## $game_indices[[19]]$version
    ## $game_indices[[19]]$version$name
    ## [1] "blue"
    ## 
    ## $game_indices[[19]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/2/"
    ## 
    ## 
    ## 
    ## $game_indices[[20]]
    ## $game_indices[[20]]$game_index
    ## [1] 153
    ## 
    ## $game_indices[[20]]$version
    ## $game_indices[[20]]$version$name
    ## [1] "red"
    ## 
    ## $game_indices[[20]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/1/"
    ## 
    ## 
    ## 
    ## 
    ## $height
    ## [1] 7
    ## 
    ## $held_items
    ## list()
    ## 
    ## $id
    ## [1] 1
    ## 
    ## $is_default
    ## [1] TRUE
    ## 
    ## $location_area_encounters
    ## [1] "https://pokeapi.co/api/v2/pokemon/1/encounters"
    ## 
    ## $moves
    ## $moves[[1]]
    ## $moves[[1]]$move
    ## $moves[[1]]$move$name
    ## [1] "razor-wind"
    ## 
    ## $moves[[1]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/13/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details
    ## $moves[[1]]$version_group_details[[1]]
    ## $moves[[1]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[1]]$version_group
    ## $moves[[1]]$version_group_details[[1]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[1]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[2]]
    ## $moves[[1]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[2]]$version_group
    ## $moves[[1]]$version_group_details[[2]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[1]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[2]]
    ## $moves[[2]]$move
    ## $moves[[2]]$move$name
    ## [1] "swords-dance"
    ## 
    ## $moves[[2]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/14/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details
    ## $moves[[2]]$version_group_details[[1]]
    ## $moves[[2]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[1]]$move_learn_method
    ## $moves[[2]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[1]]$version_group
    ## $moves[[2]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[2]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[2]]
    ## $moves[[2]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[2]]$move_learn_method
    ## $moves[[2]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[2]]$version_group
    ## $moves[[2]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[2]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[3]]
    ## $moves[[2]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[3]]$move_learn_method
    ## $moves[[2]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[3]]$version_group
    ## $moves[[2]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[2]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[4]]
    ## $moves[[2]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[4]]$move_learn_method
    ## $moves[[2]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[4]]$version_group
    ## $moves[[2]]$version_group_details[[4]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[2]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[5]]
    ## $moves[[2]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[5]]$move_learn_method
    ## $moves[[2]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[5]]$version_group
    ## $moves[[2]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[2]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[6]]
    ## $moves[[2]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[6]]$move_learn_method
    ## $moves[[2]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[6]]$version_group
    ## $moves[[2]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[2]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[7]]
    ## $moves[[2]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[7]]$move_learn_method
    ## $moves[[2]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[7]]$version_group
    ## $moves[[2]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[2]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[8]]
    ## $moves[[2]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[8]]$move_learn_method
    ## $moves[[2]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[8]]$version_group
    ## $moves[[2]]$version_group_details[[8]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[2]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[9]]
    ## $moves[[2]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[9]]$move_learn_method
    ## $moves[[2]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[9]]$version_group
    ## $moves[[2]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[2]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[10]]
    ## $moves[[2]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[10]]$move_learn_method
    ## $moves[[2]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[2]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[10]]$version_group
    ## $moves[[2]]$version_group_details[[10]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[2]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[11]]
    ## $moves[[2]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[11]]$move_learn_method
    ## $moves[[2]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[2]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[11]]$version_group
    ## $moves[[2]]$version_group_details[[11]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[2]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[12]]
    ## $moves[[2]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[12]]$move_learn_method
    ## $moves[[2]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[12]]$version_group
    ## $moves[[2]]$version_group_details[[12]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[2]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[13]]
    ## $moves[[2]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[2]]$version_group_details[[13]]$move_learn_method
    ## $moves[[2]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[2]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[2]]$version_group_details[[13]]$version_group
    ## $moves[[2]]$version_group_details[[13]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[2]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[3]]
    ## $moves[[3]]$move
    ## $moves[[3]]$move$name
    ## [1] "cut"
    ## 
    ## $moves[[3]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/15/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details
    ## $moves[[3]]$version_group_details[[1]]
    ## $moves[[3]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[1]]$move_learn_method
    ## $moves[[3]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[1]]$version_group
    ## $moves[[3]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[3]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[2]]
    ## $moves[[3]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[2]]$move_learn_method
    ## $moves[[3]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[2]]$version_group
    ## $moves[[3]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[3]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[3]]
    ## $moves[[3]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[3]]$move_learn_method
    ## $moves[[3]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[3]]$version_group
    ## $moves[[3]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[3]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[4]]
    ## $moves[[3]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[4]]$move_learn_method
    ## $moves[[3]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[4]]$version_group
    ## $moves[[3]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[3]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[5]]
    ## $moves[[3]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[5]]$move_learn_method
    ## $moves[[3]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[5]]$version_group
    ## $moves[[3]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[3]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[6]]
    ## $moves[[3]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[6]]$move_learn_method
    ## $moves[[3]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[6]]$version_group
    ## $moves[[3]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[3]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[7]]
    ## $moves[[3]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[7]]$move_learn_method
    ## $moves[[3]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[7]]$version_group
    ## $moves[[3]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[3]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[8]]
    ## $moves[[3]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[8]]$move_learn_method
    ## $moves[[3]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[8]]$version_group
    ## $moves[[3]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[3]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[9]]
    ## $moves[[3]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[9]]$move_learn_method
    ## $moves[[3]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[9]]$version_group
    ## $moves[[3]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[3]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[10]]
    ## $moves[[3]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[10]]$move_learn_method
    ## $moves[[3]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[10]]$version_group
    ## $moves[[3]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[3]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[11]]
    ## $moves[[3]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[11]]$move_learn_method
    ## $moves[[3]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[11]]$version_group
    ## $moves[[3]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[3]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[12]]
    ## $moves[[3]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[12]]$move_learn_method
    ## $moves[[3]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[12]]$version_group
    ## $moves[[3]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[3]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[13]]
    ## $moves[[3]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[13]]$move_learn_method
    ## $moves[[3]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[13]]$version_group
    ## $moves[[3]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[3]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[14]]
    ## $moves[[3]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[14]]$move_learn_method
    ## $moves[[3]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[14]]$version_group
    ## $moves[[3]]$version_group_details[[14]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[3]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[15]]
    ## $moves[[3]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[15]]$move_learn_method
    ## $moves[[3]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[15]]$version_group
    ## $moves[[3]]$version_group_details[[15]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[3]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[16]]
    ## $moves[[3]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[3]]$version_group_details[[16]]$move_learn_method
    ## $moves[[3]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[3]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[3]]$version_group_details[[16]]$version_group
    ## $moves[[3]]$version_group_details[[16]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[3]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[4]]
    ## $moves[[4]]$move
    ## $moves[[4]]$move$name
    ## [1] "bind"
    ## 
    ## $moves[[4]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/20/"
    ## 
    ## 
    ## $moves[[4]]$version_group_details
    ## $moves[[4]]$version_group_details[[1]]
    ## $moves[[4]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[4]]$version_group_details[[1]]$move_learn_method
    ## $moves[[4]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[4]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[4]]$version_group_details[[1]]$version_group
    ## $moves[[4]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[4]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[4]]$version_group_details[[2]]
    ## $moves[[4]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[4]]$version_group_details[[2]]$move_learn_method
    ## $moves[[4]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[4]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[4]]$version_group_details[[2]]$version_group
    ## $moves[[4]]$version_group_details[[2]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[4]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[5]]
    ## $moves[[5]]$move
    ## $moves[[5]]$move$name
    ## [1] "vine-whip"
    ## 
    ## $moves[[5]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/22/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details
    ## $moves[[5]]$version_group_details[[1]]
    ## $moves[[5]]$version_group_details[[1]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[1]]$move_learn_method
    ## $moves[[5]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[1]]$version_group
    ## $moves[[5]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[5]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[2]]
    ## $moves[[5]]$version_group_details[[2]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[2]]$move_learn_method
    ## $moves[[5]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[2]]$version_group
    ## $moves[[5]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[5]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[3]]
    ## $moves[[5]]$version_group_details[[3]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[3]]$move_learn_method
    ## $moves[[5]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[3]]$version_group
    ## $moves[[5]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[5]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[4]]
    ## $moves[[5]]$version_group_details[[4]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[4]]$move_learn_method
    ## $moves[[5]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[4]]$version_group
    ## $moves[[5]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[5]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[5]]
    ## $moves[[5]]$version_group_details[[5]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[5]]$move_learn_method
    ## $moves[[5]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[5]]$version_group
    ## $moves[[5]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[5]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[6]]
    ## $moves[[5]]$version_group_details[[6]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[6]]$move_learn_method
    ## $moves[[5]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[6]]$version_group
    ## $moves[[5]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[5]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[7]]
    ## $moves[[5]]$version_group_details[[7]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[7]]$move_learn_method
    ## $moves[[5]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[7]]$version_group
    ## $moves[[5]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[5]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[8]]
    ## $moves[[5]]$version_group_details[[8]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[8]]$move_learn_method
    ## $moves[[5]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[8]]$version_group
    ## $moves[[5]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[5]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[9]]
    ## $moves[[5]]$version_group_details[[9]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[9]]$move_learn_method
    ## $moves[[5]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[9]]$version_group
    ## $moves[[5]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[5]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[10]]
    ## $moves[[5]]$version_group_details[[10]]$level_learned_at
    ## [1] 9
    ## 
    ## $moves[[5]]$version_group_details[[10]]$move_learn_method
    ## $moves[[5]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[10]]$version_group
    ## $moves[[5]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[5]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[11]]
    ## $moves[[5]]$version_group_details[[11]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[5]]$version_group_details[[11]]$move_learn_method
    ## $moves[[5]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[11]]$version_group
    ## $moves[[5]]$version_group_details[[11]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[5]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[12]]
    ## $moves[[5]]$version_group_details[[12]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[12]]$move_learn_method
    ## $moves[[5]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[12]]$version_group
    ## $moves[[5]]$version_group_details[[12]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[5]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[13]]
    ## $moves[[5]]$version_group_details[[13]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[13]]$move_learn_method
    ## $moves[[5]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[13]]$version_group
    ## $moves[[5]]$version_group_details[[13]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[5]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[14]]
    ## $moves[[5]]$version_group_details[[14]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[14]]$move_learn_method
    ## $moves[[5]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[14]]$version_group
    ## $moves[[5]]$version_group_details[[14]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[5]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[15]]
    ## $moves[[5]]$version_group_details[[15]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[15]]$move_learn_method
    ## $moves[[5]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[15]]$version_group
    ## $moves[[5]]$version_group_details[[15]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[5]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[16]]
    ## $moves[[5]]$version_group_details[[16]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[16]]$move_learn_method
    ## $moves[[5]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[16]]$version_group
    ## $moves[[5]]$version_group_details[[16]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[5]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[17]]
    ## $moves[[5]]$version_group_details[[17]]$level_learned_at
    ## [1] 10
    ## 
    ## $moves[[5]]$version_group_details[[17]]$move_learn_method
    ## $moves[[5]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[17]]$version_group
    ## $moves[[5]]$version_group_details[[17]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[5]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[18]]
    ## $moves[[5]]$version_group_details[[18]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[5]]$version_group_details[[18]]$move_learn_method
    ## $moves[[5]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[18]]$version_group
    ## $moves[[5]]$version_group_details[[18]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[5]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[19]]
    ## $moves[[5]]$version_group_details[[19]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[5]]$version_group_details[[19]]$move_learn_method
    ## $moves[[5]]$version_group_details[[19]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[5]]$version_group_details[[19]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[5]]$version_group_details[[19]]$version_group
    ## $moves[[5]]$version_group_details[[19]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[5]]$version_group_details[[19]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[6]]
    ## $moves[[6]]$move
    ## $moves[[6]]$move$name
    ## [1] "headbutt"
    ## 
    ## $moves[[6]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/29/"
    ## 
    ## 
    ## $moves[[6]]$version_group_details
    ## $moves[[6]]$version_group_details[[1]]
    ## $moves[[6]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[6]]$version_group_details[[1]]$move_learn_method
    ## $moves[[6]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[6]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[6]]$version_group_details[[1]]$version_group
    ## $moves[[6]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[6]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[6]]$version_group_details[[2]]
    ## $moves[[6]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[6]]$version_group_details[[2]]$move_learn_method
    ## $moves[[6]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[6]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[6]]$version_group_details[[2]]$version_group
    ## $moves[[6]]$version_group_details[[2]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[6]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[6]]$version_group_details[[3]]
    ## $moves[[6]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[6]]$version_group_details[[3]]$move_learn_method
    ## $moves[[6]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[6]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[6]]$version_group_details[[3]]$version_group
    ## $moves[[6]]$version_group_details[[3]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[6]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[7]]
    ## $moves[[7]]$move
    ## $moves[[7]]$move$name
    ## [1] "tackle"
    ## 
    ## $moves[[7]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/33/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details
    ## $moves[[7]]$version_group_details[[1]]
    ## $moves[[7]]$version_group_details[[1]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[1]]$move_learn_method
    ## $moves[[7]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[1]]$version_group
    ## $moves[[7]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[7]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[2]]
    ## $moves[[7]]$version_group_details[[2]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[2]]$move_learn_method
    ## $moves[[7]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[2]]$version_group
    ## $moves[[7]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[7]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[3]]
    ## $moves[[7]]$version_group_details[[3]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[3]]$move_learn_method
    ## $moves[[7]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[3]]$version_group
    ## $moves[[7]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[7]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[4]]
    ## $moves[[7]]$version_group_details[[4]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[4]]$move_learn_method
    ## $moves[[7]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[4]]$version_group
    ## $moves[[7]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[7]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[5]]
    ## $moves[[7]]$version_group_details[[5]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[5]]$move_learn_method
    ## $moves[[7]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[5]]$version_group
    ## $moves[[7]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[7]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[6]]
    ## $moves[[7]]$version_group_details[[6]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[6]]$move_learn_method
    ## $moves[[7]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[6]]$version_group
    ## $moves[[7]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[7]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[7]]
    ## $moves[[7]]$version_group_details[[7]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[7]]$move_learn_method
    ## $moves[[7]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[7]]$version_group
    ## $moves[[7]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[7]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[8]]
    ## $moves[[7]]$version_group_details[[8]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[8]]$move_learn_method
    ## $moves[[7]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[8]]$version_group
    ## $moves[[7]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[7]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[9]]
    ## $moves[[7]]$version_group_details[[9]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[9]]$move_learn_method
    ## $moves[[7]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[9]]$version_group
    ## $moves[[7]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[7]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[10]]
    ## $moves[[7]]$version_group_details[[10]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[10]]$move_learn_method
    ## $moves[[7]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[10]]$version_group
    ## $moves[[7]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[7]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[11]]
    ## $moves[[7]]$version_group_details[[11]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[11]]$move_learn_method
    ## $moves[[7]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[11]]$version_group
    ## $moves[[7]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[7]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[12]]
    ## $moves[[7]]$version_group_details[[12]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[12]]$move_learn_method
    ## $moves[[7]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[12]]$version_group
    ## $moves[[7]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[7]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[13]]
    ## $moves[[7]]$version_group_details[[13]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[13]]$move_learn_method
    ## $moves[[7]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[13]]$version_group
    ## $moves[[7]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[7]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[14]]
    ## $moves[[7]]$version_group_details[[14]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[14]]$move_learn_method
    ## $moves[[7]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[14]]$version_group
    ## $moves[[7]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[7]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[15]]
    ## $moves[[7]]$version_group_details[[15]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[15]]$move_learn_method
    ## $moves[[7]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[15]]$version_group
    ## $moves[[7]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[7]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[16]]
    ## $moves[[7]]$version_group_details[[16]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[16]]$move_learn_method
    ## $moves[[7]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[16]]$version_group
    ## $moves[[7]]$version_group_details[[16]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[7]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[17]]
    ## $moves[[7]]$version_group_details[[17]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[17]]$move_learn_method
    ## $moves[[7]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[17]]$version_group
    ## $moves[[7]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[7]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[18]]
    ## $moves[[7]]$version_group_details[[18]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[7]]$version_group_details[[18]]$move_learn_method
    ## $moves[[7]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[7]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[7]]$version_group_details[[18]]$version_group
    ## $moves[[7]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[7]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[8]]
    ## $moves[[8]]$move
    ## $moves[[8]]$move$name
    ## [1] "body-slam"
    ## 
    ## $moves[[8]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/34/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details
    ## $moves[[8]]$version_group_details[[1]]
    ## $moves[[8]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[8]]$version_group_details[[1]]$move_learn_method
    ## $moves[[8]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[8]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[1]]$version_group
    ## $moves[[8]]$version_group_details[[1]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[8]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[2]]
    ## $moves[[8]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[8]]$version_group_details[[2]]$move_learn_method
    ## $moves[[8]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[8]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[2]]$version_group
    ## $moves[[8]]$version_group_details[[2]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[8]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[3]]
    ## $moves[[8]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[8]]$version_group_details[[3]]$move_learn_method
    ## $moves[[8]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[8]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[3]]$version_group
    ## $moves[[8]]$version_group_details[[3]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[8]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[4]]
    ## $moves[[8]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[8]]$version_group_details[[4]]$move_learn_method
    ## $moves[[8]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[8]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[4]]$version_group
    ## $moves[[8]]$version_group_details[[4]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[8]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[5]]
    ## $moves[[8]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[8]]$version_group_details[[5]]$move_learn_method
    ## $moves[[8]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[8]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[8]]$version_group_details[[5]]$version_group
    ## $moves[[8]]$version_group_details[[5]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[8]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[9]]
    ## $moves[[9]]$move
    ## $moves[[9]]$move$name
    ## [1] "take-down"
    ## 
    ## $moves[[9]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/36/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details
    ## $moves[[9]]$version_group_details[[1]]
    ## $moves[[9]]$version_group_details[[1]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[1]]$move_learn_method
    ## $moves[[9]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[1]]$version_group
    ## $moves[[9]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[9]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[2]]
    ## $moves[[9]]$version_group_details[[2]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[2]]$move_learn_method
    ## $moves[[9]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[2]]$version_group
    ## $moves[[9]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[9]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[3]]
    ## $moves[[9]]$version_group_details[[3]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[3]]$move_learn_method
    ## $moves[[9]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[3]]$version_group
    ## $moves[[9]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[9]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[4]]
    ## $moves[[9]]$version_group_details[[4]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[4]]$move_learn_method
    ## $moves[[9]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[4]]$version_group
    ## $moves[[9]]$version_group_details[[4]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[9]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[5]]
    ## $moves[[9]]$version_group_details[[5]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[5]]$move_learn_method
    ## $moves[[9]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[5]]$version_group
    ## $moves[[9]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[9]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[6]]
    ## $moves[[9]]$version_group_details[[6]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[6]]$move_learn_method
    ## $moves[[9]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[6]]$version_group
    ## $moves[[9]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[9]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[7]]
    ## $moves[[9]]$version_group_details[[7]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[7]]$move_learn_method
    ## $moves[[9]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[7]]$version_group
    ## $moves[[9]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[9]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[8]]
    ## $moves[[9]]$version_group_details[[8]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[8]]$move_learn_method
    ## $moves[[9]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[8]]$version_group
    ## $moves[[9]]$version_group_details[[8]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[9]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[9]]
    ## $moves[[9]]$version_group_details[[9]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[9]]$version_group_details[[9]]$move_learn_method
    ## $moves[[9]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[9]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[9]]$version_group
    ## $moves[[9]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[9]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[10]]
    ## $moves[[9]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[9]]$version_group_details[[10]]$move_learn_method
    ## $moves[[9]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[9]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[10]]$version_group
    ## $moves[[9]]$version_group_details[[10]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[9]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[11]]
    ## $moves[[9]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[9]]$version_group_details[[11]]$move_learn_method
    ## $moves[[9]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[9]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[9]]$version_group_details[[11]]$version_group
    ## $moves[[9]]$version_group_details[[11]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[9]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[10]]
    ## $moves[[10]]$move
    ## $moves[[10]]$move$name
    ## [1] "double-edge"
    ## 
    ## $moves[[10]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/38/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details
    ## $moves[[10]]$version_group_details[[1]]
    ## $moves[[10]]$version_group_details[[1]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[1]]$move_learn_method
    ## $moves[[10]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[1]]$version_group
    ## $moves[[10]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[10]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[2]]
    ## $moves[[10]]$version_group_details[[2]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[2]]$move_learn_method
    ## $moves[[10]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[2]]$version_group
    ## $moves[[10]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[10]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[3]]
    ## $moves[[10]]$version_group_details[[3]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[3]]$move_learn_method
    ## $moves[[10]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[3]]$version_group
    ## $moves[[10]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[10]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[4]]
    ## $moves[[10]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[10]]$version_group_details[[4]]$move_learn_method
    ## $moves[[10]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[10]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[4]]$version_group
    ## $moves[[10]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[10]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[5]]
    ## $moves[[10]]$version_group_details[[5]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[5]]$move_learn_method
    ## $moves[[10]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[5]]$version_group
    ## $moves[[10]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[10]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[6]]
    ## $moves[[10]]$version_group_details[[6]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[6]]$move_learn_method
    ## $moves[[10]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[6]]$version_group
    ## $moves[[10]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[10]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[7]]
    ## $moves[[10]]$version_group_details[[7]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[7]]$move_learn_method
    ## $moves[[10]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[7]]$version_group
    ## $moves[[10]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[10]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[8]]
    ## $moves[[10]]$version_group_details[[8]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[8]]$move_learn_method
    ## $moves[[10]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[8]]$version_group
    ## $moves[[10]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[10]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[9]]
    ## $moves[[10]]$version_group_details[[9]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[9]]$move_learn_method
    ## $moves[[10]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[9]]$version_group
    ## $moves[[10]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[10]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[10]]
    ## $moves[[10]]$version_group_details[[10]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[10]]$version_group_details[[10]]$move_learn_method
    ## $moves[[10]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[10]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[10]]$version_group
    ## $moves[[10]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[10]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[11]]
    ## $moves[[10]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[10]]$version_group_details[[11]]$move_learn_method
    ## $moves[[10]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[10]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[11]]$version_group
    ## $moves[[10]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[10]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[12]]
    ## $moves[[10]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[10]]$version_group_details[[12]]$move_learn_method
    ## $moves[[10]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[10]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[12]]$version_group
    ## $moves[[10]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[10]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[13]]
    ## $moves[[10]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[10]]$version_group_details[[13]]$move_learn_method
    ## $moves[[10]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[10]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[13]]$version_group
    ## $moves[[10]]$version_group_details[[13]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[10]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[14]]
    ## $moves[[10]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[10]]$version_group_details[[14]]$move_learn_method
    ## $moves[[10]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[10]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[10]]$version_group_details[[14]]$version_group
    ## $moves[[10]]$version_group_details[[14]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[10]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[11]]
    ## $moves[[11]]$move
    ## $moves[[11]]$move$name
    ## [1] "growl"
    ## 
    ## $moves[[11]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/45/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details
    ## $moves[[11]]$version_group_details[[1]]
    ## $moves[[11]]$version_group_details[[1]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[1]]$move_learn_method
    ## $moves[[11]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[1]]$version_group
    ## $moves[[11]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[11]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[2]]
    ## $moves[[11]]$version_group_details[[2]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[2]]$move_learn_method
    ## $moves[[11]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[2]]$version_group
    ## $moves[[11]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[11]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[3]]
    ## $moves[[11]]$version_group_details[[3]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[3]]$move_learn_method
    ## $moves[[11]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[3]]$version_group
    ## $moves[[11]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[11]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[4]]
    ## $moves[[11]]$version_group_details[[4]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[4]]$move_learn_method
    ## $moves[[11]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[4]]$version_group
    ## $moves[[11]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[11]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[5]]
    ## $moves[[11]]$version_group_details[[5]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[5]]$move_learn_method
    ## $moves[[11]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[5]]$version_group
    ## $moves[[11]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[11]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[6]]
    ## $moves[[11]]$version_group_details[[6]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[6]]$move_learn_method
    ## $moves[[11]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[6]]$version_group
    ## $moves[[11]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[11]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[7]]
    ## $moves[[11]]$version_group_details[[7]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[7]]$move_learn_method
    ## $moves[[11]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[7]]$version_group
    ## $moves[[11]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[11]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[8]]
    ## $moves[[11]]$version_group_details[[8]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[8]]$move_learn_method
    ## $moves[[11]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[8]]$version_group
    ## $moves[[11]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[11]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[9]]
    ## $moves[[11]]$version_group_details[[9]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[9]]$move_learn_method
    ## $moves[[11]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[9]]$version_group
    ## $moves[[11]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[11]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[10]]
    ## $moves[[11]]$version_group_details[[10]]$level_learned_at
    ## [1] 3
    ## 
    ## $moves[[11]]$version_group_details[[10]]$move_learn_method
    ## $moves[[11]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[10]]$version_group
    ## $moves[[11]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[11]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[11]]
    ## $moves[[11]]$version_group_details[[11]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[11]]$move_learn_method
    ## $moves[[11]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[11]]$version_group
    ## $moves[[11]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[11]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[12]]
    ## $moves[[11]]$version_group_details[[12]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[12]]$move_learn_method
    ## $moves[[11]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[12]]$version_group
    ## $moves[[11]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[11]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[13]]
    ## $moves[[11]]$version_group_details[[13]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[13]]$move_learn_method
    ## $moves[[11]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[13]]$version_group
    ## $moves[[11]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[11]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[14]]
    ## $moves[[11]]$version_group_details[[14]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[14]]$move_learn_method
    ## $moves[[11]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[14]]$version_group
    ## $moves[[11]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[11]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[15]]
    ## $moves[[11]]$version_group_details[[15]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[15]]$move_learn_method
    ## $moves[[11]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[15]]$version_group
    ## $moves[[11]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[11]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[16]]
    ## $moves[[11]]$version_group_details[[16]]$level_learned_at
    ## [1] 4
    ## 
    ## $moves[[11]]$version_group_details[[16]]$move_learn_method
    ## $moves[[11]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[16]]$version_group
    ## $moves[[11]]$version_group_details[[16]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[11]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[17]]
    ## $moves[[11]]$version_group_details[[17]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[11]]$version_group_details[[17]]$move_learn_method
    ## $moves[[11]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[17]]$version_group
    ## $moves[[11]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[11]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[18]]
    ## $moves[[11]]$version_group_details[[18]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[11]]$version_group_details[[18]]$move_learn_method
    ## $moves[[11]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[11]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[11]]$version_group_details[[18]]$version_group
    ## $moves[[11]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[11]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[12]]
    ## $moves[[12]]$move
    ## $moves[[12]]$move$name
    ## [1] "strength"
    ## 
    ## $moves[[12]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/70/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details
    ## $moves[[12]]$version_group_details[[1]]
    ## $moves[[12]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[1]]$move_learn_method
    ## $moves[[12]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[1]]$version_group
    ## $moves[[12]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[12]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[2]]
    ## $moves[[12]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[2]]$move_learn_method
    ## $moves[[12]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[2]]$version_group
    ## $moves[[12]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[12]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[3]]
    ## $moves[[12]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[3]]$move_learn_method
    ## $moves[[12]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[3]]$version_group
    ## $moves[[12]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[12]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[4]]
    ## $moves[[12]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[4]]$move_learn_method
    ## $moves[[12]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[4]]$version_group
    ## $moves[[12]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[12]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[5]]
    ## $moves[[12]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[5]]$move_learn_method
    ## $moves[[12]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[5]]$version_group
    ## $moves[[12]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[12]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[6]]
    ## $moves[[12]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[6]]$move_learn_method
    ## $moves[[12]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[6]]$version_group
    ## $moves[[12]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[12]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[7]]
    ## $moves[[12]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[7]]$move_learn_method
    ## $moves[[12]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[7]]$version_group
    ## $moves[[12]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[12]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[8]]
    ## $moves[[12]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[8]]$move_learn_method
    ## $moves[[12]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[8]]$version_group
    ## $moves[[12]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[12]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[9]]
    ## $moves[[12]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[9]]$move_learn_method
    ## $moves[[12]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[9]]$version_group
    ## $moves[[12]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[12]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[10]]
    ## $moves[[12]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[10]]$move_learn_method
    ## $moves[[12]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[10]]$version_group
    ## $moves[[12]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[12]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[11]]
    ## $moves[[12]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[11]]$move_learn_method
    ## $moves[[12]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[11]]$version_group
    ## $moves[[12]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[12]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[12]]
    ## $moves[[12]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[12]]$version_group_details[[12]]$move_learn_method
    ## $moves[[12]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[12]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[12]]$version_group_details[[12]]$version_group
    ## $moves[[12]]$version_group_details[[12]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[12]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[13]]
    ## $moves[[13]]$move
    ## $moves[[13]]$move$name
    ## [1] "mega-drain"
    ## 
    ## $moves[[13]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/72/"
    ## 
    ## 
    ## $moves[[13]]$version_group_details
    ## $moves[[13]]$version_group_details[[1]]
    ## $moves[[13]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[13]]$version_group_details[[1]]$move_learn_method
    ## $moves[[13]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[13]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[13]]$version_group_details[[1]]$version_group
    ## $moves[[13]]$version_group_details[[1]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[13]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[13]]$version_group_details[[2]]
    ## $moves[[13]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[13]]$version_group_details[[2]]$move_learn_method
    ## $moves[[13]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[13]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[13]]$version_group_details[[2]]$version_group
    ## $moves[[13]]$version_group_details[[2]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[13]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[14]]
    ## $moves[[14]]$move
    ## $moves[[14]]$move$name
    ## [1] "leech-seed"
    ## 
    ## $moves[[14]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/73/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details
    ## $moves[[14]]$version_group_details[[1]]
    ## $moves[[14]]$version_group_details[[1]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[1]]$move_learn_method
    ## $moves[[14]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[1]]$version_group
    ## $moves[[14]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[14]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[2]]
    ## $moves[[14]]$version_group_details[[2]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[2]]$move_learn_method
    ## $moves[[14]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[2]]$version_group
    ## $moves[[14]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[14]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[3]]
    ## $moves[[14]]$version_group_details[[3]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[3]]$move_learn_method
    ## $moves[[14]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[3]]$version_group
    ## $moves[[14]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[14]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[4]]
    ## $moves[[14]]$version_group_details[[4]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[4]]$move_learn_method
    ## $moves[[14]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[4]]$version_group
    ## $moves[[14]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[14]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[5]]
    ## $moves[[14]]$version_group_details[[5]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[5]]$move_learn_method
    ## $moves[[14]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[5]]$version_group
    ## $moves[[14]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[14]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[6]]
    ## $moves[[14]]$version_group_details[[6]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[6]]$move_learn_method
    ## $moves[[14]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[6]]$version_group
    ## $moves[[14]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[14]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[7]]
    ## $moves[[14]]$version_group_details[[7]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[7]]$move_learn_method
    ## $moves[[14]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[7]]$version_group
    ## $moves[[14]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[14]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[8]]
    ## $moves[[14]]$version_group_details[[8]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[8]]$move_learn_method
    ## $moves[[14]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[8]]$version_group
    ## $moves[[14]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[14]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[9]]
    ## $moves[[14]]$version_group_details[[9]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[9]]$move_learn_method
    ## $moves[[14]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[9]]$version_group
    ## $moves[[14]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[14]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[10]]
    ## $moves[[14]]$version_group_details[[10]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[10]]$move_learn_method
    ## $moves[[14]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[10]]$version_group
    ## $moves[[14]]$version_group_details[[10]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[14]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[11]]
    ## $moves[[14]]$version_group_details[[11]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[11]]$move_learn_method
    ## $moves[[14]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[11]]$version_group
    ## $moves[[14]]$version_group_details[[11]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[14]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[12]]
    ## $moves[[14]]$version_group_details[[12]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[12]]$move_learn_method
    ## $moves[[14]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[12]]$version_group
    ## $moves[[14]]$version_group_details[[12]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[14]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[13]]
    ## $moves[[14]]$version_group_details[[13]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[13]]$move_learn_method
    ## $moves[[14]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[13]]$version_group
    ## $moves[[14]]$version_group_details[[13]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[14]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[14]]
    ## $moves[[14]]$version_group_details[[14]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[14]]$move_learn_method
    ## $moves[[14]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[14]]$version_group
    ## $moves[[14]]$version_group_details[[14]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[14]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[15]]
    ## $moves[[14]]$version_group_details[[15]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[15]]$move_learn_method
    ## $moves[[14]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[15]]$version_group
    ## $moves[[14]]$version_group_details[[15]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[14]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[16]]
    ## $moves[[14]]$version_group_details[[16]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[16]]$move_learn_method
    ## $moves[[14]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[16]]$version_group
    ## $moves[[14]]$version_group_details[[16]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[14]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[17]]
    ## $moves[[14]]$version_group_details[[17]]$level_learned_at
    ## [1] 7
    ## 
    ## $moves[[14]]$version_group_details[[17]]$move_learn_method
    ## $moves[[14]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[14]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[14]]$version_group_details[[17]]$version_group
    ## $moves[[14]]$version_group_details[[17]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[14]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[15]]
    ## $moves[[15]]$move
    ## $moves[[15]]$move$name
    ## [1] "growth"
    ## 
    ## $moves[[15]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/74/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details
    ## $moves[[15]]$version_group_details[[1]]
    ## $moves[[15]]$version_group_details[[1]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[1]]$move_learn_method
    ## $moves[[15]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[1]]$version_group
    ## $moves[[15]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[15]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[2]]
    ## $moves[[15]]$version_group_details[[2]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[2]]$move_learn_method
    ## $moves[[15]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[2]]$version_group
    ## $moves[[15]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[15]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[3]]
    ## $moves[[15]]$version_group_details[[3]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[3]]$move_learn_method
    ## $moves[[15]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[3]]$version_group
    ## $moves[[15]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[15]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[4]]
    ## $moves[[15]]$version_group_details[[4]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[4]]$move_learn_method
    ## $moves[[15]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[4]]$version_group
    ## $moves[[15]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[15]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[5]]
    ## $moves[[15]]$version_group_details[[5]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[5]]$move_learn_method
    ## $moves[[15]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[5]]$version_group
    ## $moves[[15]]$version_group_details[[5]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[15]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[6]]
    ## $moves[[15]]$version_group_details[[6]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[6]]$move_learn_method
    ## $moves[[15]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[6]]$version_group
    ## $moves[[15]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[15]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[7]]
    ## $moves[[15]]$version_group_details[[7]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[7]]$move_learn_method
    ## $moves[[15]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[7]]$version_group
    ## $moves[[15]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[15]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[8]]
    ## $moves[[15]]$version_group_details[[8]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[8]]$move_learn_method
    ## $moves[[15]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[8]]$version_group
    ## $moves[[15]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[15]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[9]]
    ## $moves[[15]]$version_group_details[[9]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[9]]$move_learn_method
    ## $moves[[15]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[9]]$version_group
    ## $moves[[15]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[15]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[10]]
    ## $moves[[15]]$version_group_details[[10]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[15]]$version_group_details[[10]]$move_learn_method
    ## $moves[[15]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[10]]$version_group
    ## $moves[[15]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[15]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[11]]
    ## $moves[[15]]$version_group_details[[11]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[11]]$move_learn_method
    ## $moves[[15]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[11]]$version_group
    ## $moves[[15]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[15]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[12]]
    ## $moves[[15]]$version_group_details[[12]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[12]]$move_learn_method
    ## $moves[[15]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[12]]$version_group
    ## $moves[[15]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[15]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[13]]
    ## $moves[[15]]$version_group_details[[13]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[13]]$move_learn_method
    ## $moves[[15]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[13]]$version_group
    ## $moves[[15]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[15]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[14]]
    ## $moves[[15]]$version_group_details[[14]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[14]]$move_learn_method
    ## $moves[[15]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[14]]$version_group
    ## $moves[[15]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[15]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[15]]
    ## $moves[[15]]$version_group_details[[15]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[15]]$move_learn_method
    ## $moves[[15]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[15]]$version_group
    ## $moves[[15]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[15]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[16]]
    ## $moves[[15]]$version_group_details[[16]]$level_learned_at
    ## [1] 32
    ## 
    ## $moves[[15]]$version_group_details[[16]]$move_learn_method
    ## $moves[[15]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[16]]$version_group
    ## $moves[[15]]$version_group_details[[16]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[15]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[17]]
    ## $moves[[15]]$version_group_details[[17]]$level_learned_at
    ## [1] 34
    ## 
    ## $moves[[15]]$version_group_details[[17]]$move_learn_method
    ## $moves[[15]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[17]]$version_group
    ## $moves[[15]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[15]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[18]]
    ## $moves[[15]]$version_group_details[[18]]$level_learned_at
    ## [1] 34
    ## 
    ## $moves[[15]]$version_group_details[[18]]$move_learn_method
    ## $moves[[15]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[15]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[15]]$version_group_details[[18]]$version_group
    ## $moves[[15]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[15]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[16]]
    ## $moves[[16]]$move
    ## $moves[[16]]$move$name
    ## [1] "razor-leaf"
    ## 
    ## $moves[[16]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/75/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details
    ## $moves[[16]]$version_group_details[[1]]
    ## $moves[[16]]$version_group_details[[1]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[1]]$move_learn_method
    ## $moves[[16]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[1]]$version_group
    ## $moves[[16]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[16]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[2]]
    ## $moves[[16]]$version_group_details[[2]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[2]]$move_learn_method
    ## $moves[[16]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[2]]$version_group
    ## $moves[[16]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[16]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[3]]
    ## $moves[[16]]$version_group_details[[3]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[3]]$move_learn_method
    ## $moves[[16]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[3]]$version_group
    ## $moves[[16]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[16]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[4]]
    ## $moves[[16]]$version_group_details[[4]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[4]]$move_learn_method
    ## $moves[[16]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[4]]$version_group
    ## $moves[[16]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[16]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[5]]
    ## $moves[[16]]$version_group_details[[5]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[5]]$move_learn_method
    ## $moves[[16]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[5]]$version_group
    ## $moves[[16]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[16]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[6]]
    ## $moves[[16]]$version_group_details[[6]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[6]]$move_learn_method
    ## $moves[[16]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[6]]$version_group
    ## $moves[[16]]$version_group_details[[6]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[16]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[7]]
    ## $moves[[16]]$version_group_details[[7]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[7]]$move_learn_method
    ## $moves[[16]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[7]]$version_group
    ## $moves[[16]]$version_group_details[[7]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[16]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[8]]
    ## $moves[[16]]$version_group_details[[8]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[8]]$move_learn_method
    ## $moves[[16]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[8]]$version_group
    ## $moves[[16]]$version_group_details[[8]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[16]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[9]]
    ## $moves[[16]]$version_group_details[[9]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[9]]$move_learn_method
    ## $moves[[16]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[9]]$version_group
    ## $moves[[16]]$version_group_details[[9]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[16]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[10]]
    ## $moves[[16]]$version_group_details[[10]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[10]]$move_learn_method
    ## $moves[[16]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[10]]$version_group
    ## $moves[[16]]$version_group_details[[10]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[16]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[11]]
    ## $moves[[16]]$version_group_details[[11]]$level_learned_at
    ## [1] 19
    ## 
    ## $moves[[16]]$version_group_details[[11]]$move_learn_method
    ## $moves[[16]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[11]]$version_group
    ## $moves[[16]]$version_group_details[[11]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[16]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[12]]
    ## $moves[[16]]$version_group_details[[12]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[12]]$move_learn_method
    ## $moves[[16]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[12]]$version_group
    ## $moves[[16]]$version_group_details[[12]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[16]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[13]]
    ## $moves[[16]]$version_group_details[[13]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[13]]$move_learn_method
    ## $moves[[16]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[13]]$version_group
    ## $moves[[16]]$version_group_details[[13]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[16]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[14]]
    ## $moves[[16]]$version_group_details[[14]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[14]]$move_learn_method
    ## $moves[[16]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[14]]$version_group
    ## $moves[[16]]$version_group_details[[14]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[16]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[15]]
    ## $moves[[16]]$version_group_details[[15]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[15]]$move_learn_method
    ## $moves[[16]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[15]]$version_group
    ## $moves[[16]]$version_group_details[[15]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[16]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[16]]
    ## $moves[[16]]$version_group_details[[16]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[16]]$version_group_details[[16]]$move_learn_method
    ## $moves[[16]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[16]]$version_group
    ## $moves[[16]]$version_group_details[[16]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[16]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[17]]
    ## $moves[[16]]$version_group_details[[17]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[16]]$version_group_details[[17]]$move_learn_method
    ## $moves[[16]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[17]]$version_group
    ## $moves[[16]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[16]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[18]]
    ## $moves[[16]]$version_group_details[[18]]$level_learned_at
    ## [1] 27
    ## 
    ## $moves[[16]]$version_group_details[[18]]$move_learn_method
    ## $moves[[16]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[16]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[16]]$version_group_details[[18]]$version_group
    ## $moves[[16]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[16]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[17]]
    ## $moves[[17]]$move
    ## $moves[[17]]$move$name
    ## [1] "solar-beam"
    ## 
    ## $moves[[17]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/76/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details
    ## $moves[[17]]$version_group_details[[1]]
    ## $moves[[17]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[1]]$move_learn_method
    ## $moves[[17]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[1]]$version_group
    ## $moves[[17]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[17]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[2]]
    ## $moves[[17]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[2]]$move_learn_method
    ## $moves[[17]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[2]]$version_group
    ## $moves[[17]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[17]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[3]]
    ## $moves[[17]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[3]]$move_learn_method
    ## $moves[[17]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[3]]$version_group
    ## $moves[[17]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[17]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[4]]
    ## $moves[[17]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[4]]$move_learn_method
    ## $moves[[17]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[4]]$version_group
    ## $moves[[17]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[17]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[5]]
    ## $moves[[17]]$version_group_details[[5]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[5]]$move_learn_method
    ## $moves[[17]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[5]]$version_group
    ## $moves[[17]]$version_group_details[[5]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[17]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[6]]
    ## $moves[[17]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[6]]$move_learn_method
    ## $moves[[17]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[6]]$version_group
    ## $moves[[17]]$version_group_details[[6]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[17]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[7]]
    ## $moves[[17]]$version_group_details[[7]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[7]]$move_learn_method
    ## $moves[[17]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[7]]$version_group
    ## $moves[[17]]$version_group_details[[7]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[17]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[8]]
    ## $moves[[17]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[8]]$move_learn_method
    ## $moves[[17]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[8]]$version_group
    ## $moves[[17]]$version_group_details[[8]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[17]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[9]]
    ## $moves[[17]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[9]]$move_learn_method
    ## $moves[[17]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[9]]$version_group
    ## $moves[[17]]$version_group_details[[9]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[17]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[10]]
    ## $moves[[17]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[10]]$move_learn_method
    ## $moves[[17]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[10]]$version_group
    ## $moves[[17]]$version_group_details[[10]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[17]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[11]]
    ## $moves[[17]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[11]]$move_learn_method
    ## $moves[[17]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[11]]$version_group
    ## $moves[[17]]$version_group_details[[11]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[17]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[12]]
    ## $moves[[17]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[12]]$move_learn_method
    ## $moves[[17]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[12]]$version_group
    ## $moves[[17]]$version_group_details[[12]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[17]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[13]]
    ## $moves[[17]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[13]]$move_learn_method
    ## $moves[[17]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[13]]$version_group
    ## $moves[[17]]$version_group_details[[13]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[17]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[14]]
    ## $moves[[17]]$version_group_details[[14]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[14]]$move_learn_method
    ## $moves[[17]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[14]]$version_group
    ## $moves[[17]]$version_group_details[[14]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[17]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[15]]
    ## $moves[[17]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[15]]$move_learn_method
    ## $moves[[17]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[15]]$version_group
    ## $moves[[17]]$version_group_details[[15]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[17]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[16]]
    ## $moves[[17]]$version_group_details[[16]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[16]]$move_learn_method
    ## $moves[[17]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[16]]$version_group
    ## $moves[[17]]$version_group_details[[16]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[17]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[17]]
    ## $moves[[17]]$version_group_details[[17]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[17]]$move_learn_method
    ## $moves[[17]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[17]]$version_group
    ## $moves[[17]]$version_group_details[[17]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[17]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[18]]
    ## $moves[[17]]$version_group_details[[18]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[18]]$move_learn_method
    ## $moves[[17]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[18]]$version_group
    ## $moves[[17]]$version_group_details[[18]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[17]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[19]]
    ## $moves[[17]]$version_group_details[[19]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[19]]$move_learn_method
    ## $moves[[17]]$version_group_details[[19]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[19]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[19]]$version_group
    ## $moves[[17]]$version_group_details[[19]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[17]]$version_group_details[[19]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[20]]
    ## $moves[[17]]$version_group_details[[20]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[20]]$move_learn_method
    ## $moves[[17]]$version_group_details[[20]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[20]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[20]]$version_group
    ## $moves[[17]]$version_group_details[[20]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[17]]$version_group_details[[20]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[21]]
    ## $moves[[17]]$version_group_details[[21]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[21]]$move_learn_method
    ## $moves[[17]]$version_group_details[[21]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[21]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[21]]$version_group
    ## $moves[[17]]$version_group_details[[21]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[17]]$version_group_details[[21]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[22]]
    ## $moves[[17]]$version_group_details[[22]]$level_learned_at
    ## [1] 46
    ## 
    ## $moves[[17]]$version_group_details[[22]]$move_learn_method
    ## $moves[[17]]$version_group_details[[22]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[22]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[22]]$version_group
    ## $moves[[17]]$version_group_details[[22]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[17]]$version_group_details[[22]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[23]]
    ## $moves[[17]]$version_group_details[[23]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[23]]$move_learn_method
    ## $moves[[17]]$version_group_details[[23]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[23]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[23]]$version_group
    ## $moves[[17]]$version_group_details[[23]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[17]]$version_group_details[[23]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[24]]
    ## $moves[[17]]$version_group_details[[24]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[24]]$move_learn_method
    ## $moves[[17]]$version_group_details[[24]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[24]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[24]]$version_group
    ## $moves[[17]]$version_group_details[[24]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[17]]$version_group_details[[24]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[25]]
    ## $moves[[17]]$version_group_details[[25]]$level_learned_at
    ## [1] 48
    ## 
    ## $moves[[17]]$version_group_details[[25]]$move_learn_method
    ## $moves[[17]]$version_group_details[[25]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[25]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[25]]$version_group
    ## $moves[[17]]$version_group_details[[25]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[17]]$version_group_details[[25]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[26]]
    ## $moves[[17]]$version_group_details[[26]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[17]]$version_group_details[[26]]$move_learn_method
    ## $moves[[17]]$version_group_details[[26]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[17]]$version_group_details[[26]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[26]]$version_group
    ## $moves[[17]]$version_group_details[[26]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[17]]$version_group_details[[26]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[27]]
    ## $moves[[17]]$version_group_details[[27]]$level_learned_at
    ## [1] 48
    ## 
    ## $moves[[17]]$version_group_details[[27]]$move_learn_method
    ## $moves[[17]]$version_group_details[[27]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[17]]$version_group_details[[27]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[17]]$version_group_details[[27]]$version_group
    ## $moves[[17]]$version_group_details[[27]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[17]]$version_group_details[[27]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[18]]
    ## $moves[[18]]$move
    ## $moves[[18]]$move$name
    ## [1] "poison-powder"
    ## 
    ## $moves[[18]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/77/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details
    ## $moves[[18]]$version_group_details[[1]]
    ## $moves[[18]]$version_group_details[[1]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[1]]$move_learn_method
    ## $moves[[18]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[1]]$version_group
    ## $moves[[18]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[18]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[2]]
    ## $moves[[18]]$version_group_details[[2]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[2]]$move_learn_method
    ## $moves[[18]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[2]]$version_group
    ## $moves[[18]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[18]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[3]]
    ## $moves[[18]]$version_group_details[[3]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[3]]$move_learn_method
    ## $moves[[18]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[3]]$version_group
    ## $moves[[18]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[18]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[4]]
    ## $moves[[18]]$version_group_details[[4]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[4]]$move_learn_method
    ## $moves[[18]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[4]]$version_group
    ## $moves[[18]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[18]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[5]]
    ## $moves[[18]]$version_group_details[[5]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[5]]$move_learn_method
    ## $moves[[18]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[5]]$version_group
    ## $moves[[18]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[18]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[6]]
    ## $moves[[18]]$version_group_details[[6]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[6]]$move_learn_method
    ## $moves[[18]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[6]]$version_group
    ## $moves[[18]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[18]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[7]]
    ## $moves[[18]]$version_group_details[[7]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[7]]$move_learn_method
    ## $moves[[18]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[7]]$version_group
    ## $moves[[18]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[18]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[8]]
    ## $moves[[18]]$version_group_details[[8]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[8]]$move_learn_method
    ## $moves[[18]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[8]]$version_group
    ## $moves[[18]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[18]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[9]]
    ## $moves[[18]]$version_group_details[[9]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[9]]$move_learn_method
    ## $moves[[18]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[9]]$version_group
    ## $moves[[18]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[18]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[10]]
    ## $moves[[18]]$version_group_details[[10]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[10]]$move_learn_method
    ## $moves[[18]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[10]]$version_group
    ## $moves[[18]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[18]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[11]]
    ## $moves[[18]]$version_group_details[[11]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[11]]$move_learn_method
    ## $moves[[18]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[11]]$version_group
    ## $moves[[18]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[18]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[12]]
    ## $moves[[18]]$version_group_details[[12]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[12]]$move_learn_method
    ## $moves[[18]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[12]]$version_group
    ## $moves[[18]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[18]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[13]]
    ## $moves[[18]]$version_group_details[[13]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[13]]$move_learn_method
    ## $moves[[18]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[13]]$version_group
    ## $moves[[18]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[18]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[14]]
    ## $moves[[18]]$version_group_details[[14]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[14]]$move_learn_method
    ## $moves[[18]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[14]]$version_group
    ## $moves[[18]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[18]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[15]]
    ## $moves[[18]]$version_group_details[[15]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[18]]$version_group_details[[15]]$move_learn_method
    ## $moves[[18]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[15]]$version_group
    ## $moves[[18]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[18]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[16]]
    ## $moves[[18]]$version_group_details[[16]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[18]]$version_group_details[[16]]$move_learn_method
    ## $moves[[18]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[16]]$version_group
    ## $moves[[18]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[18]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[17]]
    ## $moves[[18]]$version_group_details[[17]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[18]]$version_group_details[[17]]$move_learn_method
    ## $moves[[18]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[17]]$version_group
    ## $moves[[18]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[18]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[18]]
    ## $moves[[18]]$version_group_details[[18]]$level_learned_at
    ## [1] 20
    ## 
    ## $moves[[18]]$version_group_details[[18]]$move_learn_method
    ## $moves[[18]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[18]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[18]]$version_group_details[[18]]$version_group
    ## $moves[[18]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[18]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[19]]
    ## $moves[[19]]$move
    ## $moves[[19]]$move$name
    ## [1] "sleep-powder"
    ## 
    ## $moves[[19]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/79/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details
    ## $moves[[19]]$version_group_details[[1]]
    ## $moves[[19]]$version_group_details[[1]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[1]]$move_learn_method
    ## $moves[[19]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[1]]$version_group
    ## $moves[[19]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[19]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[2]]
    ## $moves[[19]]$version_group_details[[2]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[2]]$move_learn_method
    ## $moves[[19]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[2]]$version_group
    ## $moves[[19]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[19]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[3]]
    ## $moves[[19]]$version_group_details[[3]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[3]]$move_learn_method
    ## $moves[[19]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[3]]$version_group
    ## $moves[[19]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[19]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[4]]
    ## $moves[[19]]$version_group_details[[4]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[4]]$move_learn_method
    ## $moves[[19]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[4]]$version_group
    ## $moves[[19]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[19]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[5]]
    ## $moves[[19]]$version_group_details[[5]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[5]]$move_learn_method
    ## $moves[[19]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[5]]$version_group
    ## $moves[[19]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[19]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[6]]
    ## $moves[[19]]$version_group_details[[6]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[6]]$move_learn_method
    ## $moves[[19]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[6]]$version_group
    ## $moves[[19]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[19]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[7]]
    ## $moves[[19]]$version_group_details[[7]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[7]]$move_learn_method
    ## $moves[[19]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[7]]$version_group
    ## $moves[[19]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[19]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[8]]
    ## $moves[[19]]$version_group_details[[8]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[8]]$move_learn_method
    ## $moves[[19]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[8]]$version_group
    ## $moves[[19]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[19]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[9]]
    ## $moves[[19]]$version_group_details[[9]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[9]]$move_learn_method
    ## $moves[[19]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[9]]$version_group
    ## $moves[[19]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[19]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[10]]
    ## $moves[[19]]$version_group_details[[10]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[10]]$move_learn_method
    ## $moves[[19]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[10]]$version_group
    ## $moves[[19]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[19]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[11]]
    ## $moves[[19]]$version_group_details[[11]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[11]]$move_learn_method
    ## $moves[[19]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[11]]$version_group
    ## $moves[[19]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[19]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[12]]
    ## $moves[[19]]$version_group_details[[12]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[12]]$move_learn_method
    ## $moves[[19]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[12]]$version_group
    ## $moves[[19]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[19]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[13]]
    ## $moves[[19]]$version_group_details[[13]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[13]]$move_learn_method
    ## $moves[[19]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[13]]$version_group
    ## $moves[[19]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[19]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[14]]
    ## $moves[[19]]$version_group_details[[14]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[14]]$move_learn_method
    ## $moves[[19]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[14]]$version_group
    ## $moves[[19]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[19]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[15]]
    ## $moves[[19]]$version_group_details[[15]]$level_learned_at
    ## [1] 15
    ## 
    ## $moves[[19]]$version_group_details[[15]]$move_learn_method
    ## $moves[[19]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[15]]$version_group
    ## $moves[[19]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[19]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[16]]
    ## $moves[[19]]$version_group_details[[16]]$level_learned_at
    ## [1] 13
    ## 
    ## $moves[[19]]$version_group_details[[16]]$move_learn_method
    ## $moves[[19]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[16]]$version_group
    ## $moves[[19]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[19]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[17]]
    ## $moves[[19]]$version_group_details[[17]]$level_learned_at
    ## [1] 41
    ## 
    ## $moves[[19]]$version_group_details[[17]]$move_learn_method
    ## $moves[[19]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[17]]$version_group
    ## $moves[[19]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[19]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[18]]
    ## $moves[[19]]$version_group_details[[18]]$level_learned_at
    ## [1] 41
    ## 
    ## $moves[[19]]$version_group_details[[18]]$move_learn_method
    ## $moves[[19]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[19]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[19]]$version_group_details[[18]]$version_group
    ## $moves[[19]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[19]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[20]]
    ## $moves[[20]]$move
    ## $moves[[20]]$move$name
    ## [1] "petal-dance"
    ## 
    ## $moves[[20]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/80/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details
    ## $moves[[20]]$version_group_details[[1]]
    ## $moves[[20]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[1]]$move_learn_method
    ## $moves[[20]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[1]]$version_group
    ## $moves[[20]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[20]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[2]]
    ## $moves[[20]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[2]]$move_learn_method
    ## $moves[[20]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[2]]$version_group
    ## $moves[[20]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[20]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[3]]
    ## $moves[[20]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[3]]$move_learn_method
    ## $moves[[20]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[3]]$version_group
    ## $moves[[20]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[20]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[4]]
    ## $moves[[20]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[4]]$move_learn_method
    ## $moves[[20]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[4]]$version_group
    ## $moves[[20]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[20]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[5]]
    ## $moves[[20]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[5]]$move_learn_method
    ## $moves[[20]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[5]]$version_group
    ## $moves[[20]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[20]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[6]]
    ## $moves[[20]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[6]]$move_learn_method
    ## $moves[[20]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[6]]$version_group
    ## $moves[[20]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[20]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[7]]
    ## $moves[[20]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[7]]$move_learn_method
    ## $moves[[20]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[7]]$version_group
    ## $moves[[20]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[20]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[8]]
    ## $moves[[20]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[8]]$move_learn_method
    ## $moves[[20]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[8]]$version_group
    ## $moves[[20]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[20]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[9]]
    ## $moves[[20]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[9]]$move_learn_method
    ## $moves[[20]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[9]]$version_group
    ## $moves[[20]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[20]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[10]]
    ## $moves[[20]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[10]]$move_learn_method
    ## $moves[[20]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[10]]$version_group
    ## $moves[[20]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[20]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[11]]
    ## $moves[[20]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[11]]$move_learn_method
    ## $moves[[20]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[11]]$version_group
    ## $moves[[20]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[20]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[12]]
    ## $moves[[20]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[12]]$move_learn_method
    ## $moves[[20]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[12]]$version_group
    ## $moves[[20]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[20]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[13]]
    ## $moves[[20]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[13]]$move_learn_method
    ## $moves[[20]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[13]]$version_group
    ## $moves[[20]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[20]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[14]]
    ## $moves[[20]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[20]]$version_group_details[[14]]$move_learn_method
    ## $moves[[20]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[20]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[20]]$version_group_details[[14]]$version_group
    ## $moves[[20]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[20]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[21]]
    ## $moves[[21]]$move
    ## $moves[[21]]$move$name
    ## [1] "string-shot"
    ## 
    ## $moves[[21]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/81/"
    ## 
    ## 
    ## $moves[[21]]$version_group_details
    ## $moves[[21]]$version_group_details[[1]]
    ## $moves[[21]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[21]]$version_group_details[[1]]$move_learn_method
    ## $moves[[21]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[21]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[21]]$version_group_details[[1]]$version_group
    ## $moves[[21]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[21]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[22]]
    ## $moves[[22]]$move
    ## $moves[[22]]$move$name
    ## [1] "toxic"
    ## 
    ## $moves[[22]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/92/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details
    ## $moves[[22]]$version_group_details[[1]]
    ## $moves[[22]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[1]]$move_learn_method
    ## $moves[[22]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[1]]$version_group
    ## $moves[[22]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[22]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[2]]
    ## $moves[[22]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[2]]$move_learn_method
    ## $moves[[22]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[2]]$version_group
    ## $moves[[22]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[22]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[3]]
    ## $moves[[22]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[3]]$move_learn_method
    ## $moves[[22]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[3]]$version_group
    ## $moves[[22]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[22]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[4]]
    ## $moves[[22]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[4]]$move_learn_method
    ## $moves[[22]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[4]]$version_group
    ## $moves[[22]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[22]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[5]]
    ## $moves[[22]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[5]]$move_learn_method
    ## $moves[[22]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[5]]$version_group
    ## $moves[[22]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[22]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[6]]
    ## $moves[[22]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[6]]$move_learn_method
    ## $moves[[22]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[6]]$version_group
    ## $moves[[22]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[22]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[7]]
    ## $moves[[22]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[7]]$move_learn_method
    ## $moves[[22]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[7]]$version_group
    ## $moves[[22]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[22]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[8]]
    ## $moves[[22]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[8]]$move_learn_method
    ## $moves[[22]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[8]]$version_group
    ## $moves[[22]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[22]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[9]]
    ## $moves[[22]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[9]]$move_learn_method
    ## $moves[[22]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[9]]$version_group
    ## $moves[[22]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[22]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[10]]
    ## $moves[[22]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[10]]$move_learn_method
    ## $moves[[22]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[10]]$version_group
    ## $moves[[22]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[22]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[11]]
    ## $moves[[22]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[11]]$move_learn_method
    ## $moves[[22]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[11]]$version_group
    ## $moves[[22]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[22]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[12]]
    ## $moves[[22]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[12]]$move_learn_method
    ## $moves[[22]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[12]]$version_group
    ## $moves[[22]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[22]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[13]]
    ## $moves[[22]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[13]]$move_learn_method
    ## $moves[[22]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[13]]$version_group
    ## $moves[[22]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[22]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[14]]
    ## $moves[[22]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[14]]$move_learn_method
    ## $moves[[22]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[14]]$version_group
    ## $moves[[22]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[22]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[15]]
    ## $moves[[22]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[15]]$move_learn_method
    ## $moves[[22]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[15]]$version_group
    ## $moves[[22]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[22]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[16]]
    ## $moves[[22]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[16]]$move_learn_method
    ## $moves[[22]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[16]]$version_group
    ## $moves[[22]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[22]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[17]]
    ## $moves[[22]]$version_group_details[[17]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[17]]$move_learn_method
    ## $moves[[22]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[17]]$version_group
    ## $moves[[22]]$version_group_details[[17]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[22]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[18]]
    ## $moves[[22]]$version_group_details[[18]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[22]]$version_group_details[[18]]$move_learn_method
    ## $moves[[22]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[22]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[22]]$version_group_details[[18]]$version_group
    ## $moves[[22]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[22]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[23]]
    ## $moves[[23]]$move
    ## $moves[[23]]$move$name
    ## [1] "rage"
    ## 
    ## $moves[[23]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/99/"
    ## 
    ## 
    ## $moves[[23]]$version_group_details
    ## $moves[[23]]$version_group_details[[1]]
    ## $moves[[23]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[23]]$version_group_details[[1]]$move_learn_method
    ## $moves[[23]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[23]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[23]]$version_group_details[[1]]$version_group
    ## $moves[[23]]$version_group_details[[1]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[23]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[23]]$version_group_details[[2]]
    ## $moves[[23]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[23]]$version_group_details[[2]]$move_learn_method
    ## $moves[[23]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[23]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[23]]$version_group_details[[2]]$version_group
    ## $moves[[23]]$version_group_details[[2]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[23]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[24]]
    ## $moves[[24]]$move
    ## $moves[[24]]$move$name
    ## [1] "mimic"
    ## 
    ## $moves[[24]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/102/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details
    ## $moves[[24]]$version_group_details[[1]]
    ## $moves[[24]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[24]]$version_group_details[[1]]$move_learn_method
    ## $moves[[24]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[24]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[1]]$version_group
    ## $moves[[24]]$version_group_details[[1]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[24]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[2]]
    ## $moves[[24]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[24]]$version_group_details[[2]]$move_learn_method
    ## $moves[[24]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[24]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[2]]$version_group
    ## $moves[[24]]$version_group_details[[2]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[24]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[3]]
    ## $moves[[24]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[24]]$version_group_details[[3]]$move_learn_method
    ## $moves[[24]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[24]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[3]]$version_group
    ## $moves[[24]]$version_group_details[[3]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[24]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[4]]
    ## $moves[[24]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[24]]$version_group_details[[4]]$move_learn_method
    ## $moves[[24]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[24]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[4]]$version_group
    ## $moves[[24]]$version_group_details[[4]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[24]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[5]]
    ## $moves[[24]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[24]]$version_group_details[[5]]$move_learn_method
    ## $moves[[24]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[24]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[24]]$version_group_details[[5]]$version_group
    ## $moves[[24]]$version_group_details[[5]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[24]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[25]]
    ## $moves[[25]]$move
    ## $moves[[25]]$move$name
    ## [1] "double-team"
    ## 
    ## $moves[[25]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/104/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details
    ## $moves[[25]]$version_group_details[[1]]
    ## $moves[[25]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[1]]$move_learn_method
    ## $moves[[25]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[1]]$version_group
    ## $moves[[25]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[25]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[2]]
    ## $moves[[25]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[2]]$move_learn_method
    ## $moves[[25]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[2]]$version_group
    ## $moves[[25]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[25]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[3]]
    ## $moves[[25]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[3]]$move_learn_method
    ## $moves[[25]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[3]]$version_group
    ## $moves[[25]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[25]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[4]]
    ## $moves[[25]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[4]]$move_learn_method
    ## $moves[[25]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[4]]$version_group
    ## $moves[[25]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[25]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[5]]
    ## $moves[[25]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[5]]$move_learn_method
    ## $moves[[25]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[5]]$version_group
    ## $moves[[25]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[25]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[6]]
    ## $moves[[25]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[6]]$move_learn_method
    ## $moves[[25]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[6]]$version_group
    ## $moves[[25]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[25]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[7]]
    ## $moves[[25]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[7]]$move_learn_method
    ## $moves[[25]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[7]]$version_group
    ## $moves[[25]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[25]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[8]]
    ## $moves[[25]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[8]]$move_learn_method
    ## $moves[[25]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[8]]$version_group
    ## $moves[[25]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[25]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[9]]
    ## $moves[[25]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[9]]$move_learn_method
    ## $moves[[25]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[9]]$version_group
    ## $moves[[25]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[25]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[10]]
    ## $moves[[25]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[10]]$move_learn_method
    ## $moves[[25]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[10]]$version_group
    ## $moves[[25]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[25]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[11]]
    ## $moves[[25]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[11]]$move_learn_method
    ## $moves[[25]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[11]]$version_group
    ## $moves[[25]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[25]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[12]]
    ## $moves[[25]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[12]]$move_learn_method
    ## $moves[[25]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[12]]$version_group
    ## $moves[[25]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[25]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[13]]
    ## $moves[[25]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[13]]$move_learn_method
    ## $moves[[25]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[13]]$version_group
    ## $moves[[25]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[25]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[14]]
    ## $moves[[25]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[14]]$move_learn_method
    ## $moves[[25]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[14]]$version_group
    ## $moves[[25]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[25]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[15]]
    ## $moves[[25]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[15]]$move_learn_method
    ## $moves[[25]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[15]]$version_group
    ## $moves[[25]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[25]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[16]]
    ## $moves[[25]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[16]]$move_learn_method
    ## $moves[[25]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[16]]$version_group
    ## $moves[[25]]$version_group_details[[16]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[25]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[17]]
    ## $moves[[25]]$version_group_details[[17]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[17]]$move_learn_method
    ## $moves[[25]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[17]]$version_group
    ## $moves[[25]]$version_group_details[[17]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[25]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[18]]
    ## $moves[[25]]$version_group_details[[18]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[25]]$version_group_details[[18]]$move_learn_method
    ## $moves[[25]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[25]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[25]]$version_group_details[[18]]$version_group
    ## $moves[[25]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[25]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[26]]
    ## $moves[[26]]$move
    ## $moves[[26]]$move$name
    ## [1] "defense-curl"
    ## 
    ## $moves[[26]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/111/"
    ## 
    ## 
    ## $moves[[26]]$version_group_details
    ## $moves[[26]]$version_group_details[[1]]
    ## $moves[[26]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[26]]$version_group_details[[1]]$move_learn_method
    ## $moves[[26]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[26]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[26]]$version_group_details[[1]]$version_group
    ## $moves[[26]]$version_group_details[[1]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[26]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[26]]$version_group_details[[2]]
    ## $moves[[26]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[26]]$version_group_details[[2]]$move_learn_method
    ## $moves[[26]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[26]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[26]]$version_group_details[[2]]$version_group
    ## $moves[[26]]$version_group_details[[2]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[26]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[26]]$version_group_details[[3]]
    ## $moves[[26]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[26]]$version_group_details[[3]]$move_learn_method
    ## $moves[[26]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[26]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[26]]$version_group_details[[3]]$version_group
    ## $moves[[26]]$version_group_details[[3]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[26]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[27]]
    ## $moves[[27]]$move
    ## $moves[[27]]$move$name
    ## [1] "light-screen"
    ## 
    ## $moves[[27]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/113/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details
    ## $moves[[27]]$version_group_details[[1]]
    ## $moves[[27]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[1]]$move_learn_method
    ## $moves[[27]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[1]]$version_group
    ## $moves[[27]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[27]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[2]]
    ## $moves[[27]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[2]]$move_learn_method
    ## $moves[[27]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[2]]$version_group
    ## $moves[[27]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[27]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[3]]
    ## $moves[[27]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[3]]$move_learn_method
    ## $moves[[27]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[3]]$version_group
    ## $moves[[27]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[27]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[4]]
    ## $moves[[27]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[4]]$move_learn_method
    ## $moves[[27]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[4]]$version_group
    ## $moves[[27]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[27]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[5]]
    ## $moves[[27]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[5]]$move_learn_method
    ## $moves[[27]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[5]]$version_group
    ## $moves[[27]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[27]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[6]]
    ## $moves[[27]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[6]]$move_learn_method
    ## $moves[[27]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[6]]$version_group
    ## $moves[[27]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[27]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[7]]
    ## $moves[[27]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[7]]$move_learn_method
    ## $moves[[27]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[7]]$version_group
    ## $moves[[27]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[27]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[8]]
    ## $moves[[27]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[8]]$move_learn_method
    ## $moves[[27]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[8]]$version_group
    ## $moves[[27]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[27]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[9]]
    ## $moves[[27]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[9]]$move_learn_method
    ## $moves[[27]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[9]]$version_group
    ## $moves[[27]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[27]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[10]]
    ## $moves[[27]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[10]]$move_learn_method
    ## $moves[[27]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[10]]$version_group
    ## $moves[[27]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[27]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[11]]
    ## $moves[[27]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[11]]$move_learn_method
    ## $moves[[27]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[11]]$version_group
    ## $moves[[27]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[27]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[12]]
    ## $moves[[27]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[12]]$move_learn_method
    ## $moves[[27]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[12]]$version_group
    ## $moves[[27]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[27]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[13]]
    ## $moves[[27]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[13]]$move_learn_method
    ## $moves[[27]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[27]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[13]]$version_group
    ## $moves[[27]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[27]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[14]]
    ## $moves[[27]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[27]]$version_group_details[[14]]$move_learn_method
    ## $moves[[27]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[27]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[27]]$version_group_details[[14]]$version_group
    ## $moves[[27]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[27]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[28]]
    ## $moves[[28]]$move
    ## $moves[[28]]$move$name
    ## [1] "reflect"
    ## 
    ## $moves[[28]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/115/"
    ## 
    ## 
    ## $moves[[28]]$version_group_details
    ## $moves[[28]]$version_group_details[[1]]
    ## $moves[[28]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[28]]$version_group_details[[1]]$move_learn_method
    ## $moves[[28]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[28]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[28]]$version_group_details[[1]]$version_group
    ## $moves[[28]]$version_group_details[[1]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[28]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[28]]$version_group_details[[2]]
    ## $moves[[28]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[28]]$version_group_details[[2]]$move_learn_method
    ## $moves[[28]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[28]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[28]]$version_group_details[[2]]$version_group
    ## $moves[[28]]$version_group_details[[2]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[28]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[29]]
    ## $moves[[29]]$move
    ## $moves[[29]]$move$name
    ## [1] "bide"
    ## 
    ## $moves[[29]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/117/"
    ## 
    ## 
    ## $moves[[29]]$version_group_details
    ## $moves[[29]]$version_group_details[[1]]
    ## $moves[[29]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[29]]$version_group_details[[1]]$move_learn_method
    ## $moves[[29]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[29]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[29]]$version_group_details[[1]]$version_group
    ## $moves[[29]]$version_group_details[[1]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[29]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[29]]$version_group_details[[2]]
    ## $moves[[29]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[29]]$version_group_details[[2]]$move_learn_method
    ## $moves[[29]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[29]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[29]]$version_group_details[[2]]$version_group
    ## $moves[[29]]$version_group_details[[2]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[29]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[30]]
    ## $moves[[30]]$move
    ## $moves[[30]]$move$name
    ## [1] "sludge"
    ## 
    ## $moves[[30]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/124/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details
    ## $moves[[30]]$version_group_details[[1]]
    ## $moves[[30]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[1]]$move_learn_method
    ## $moves[[30]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[1]]$version_group
    ## $moves[[30]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[30]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[2]]
    ## $moves[[30]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[2]]$move_learn_method
    ## $moves[[30]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[2]]$version_group
    ## $moves[[30]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[30]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[3]]
    ## $moves[[30]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[3]]$move_learn_method
    ## $moves[[30]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[3]]$version_group
    ## $moves[[30]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[30]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[4]]
    ## $moves[[30]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[4]]$move_learn_method
    ## $moves[[30]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[4]]$version_group
    ## $moves[[30]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[30]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[5]]
    ## $moves[[30]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[5]]$move_learn_method
    ## $moves[[30]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[5]]$version_group
    ## $moves[[30]]$version_group_details[[5]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[30]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[6]]
    ## $moves[[30]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[6]]$move_learn_method
    ## $moves[[30]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[6]]$version_group
    ## $moves[[30]]$version_group_details[[6]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[30]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[7]]
    ## $moves[[30]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[30]]$version_group_details[[7]]$move_learn_method
    ## $moves[[30]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[30]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[30]]$version_group_details[[7]]$version_group
    ## $moves[[30]]$version_group_details[[7]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[30]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[31]]
    ## $moves[[31]]$move
    ## $moves[[31]]$move$name
    ## [1] "skull-bash"
    ## 
    ## $moves[[31]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/130/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details
    ## $moves[[31]]$version_group_details[[1]]
    ## $moves[[31]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[1]]$move_learn_method
    ## $moves[[31]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[1]]$version_group
    ## $moves[[31]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[31]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[2]]
    ## $moves[[31]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[2]]$move_learn_method
    ## $moves[[31]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[2]]$version_group
    ## $moves[[31]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[31]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[3]]
    ## $moves[[31]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[3]]$move_learn_method
    ## $moves[[31]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[3]]$version_group
    ## $moves[[31]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[31]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[4]]
    ## $moves[[31]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[4]]$move_learn_method
    ## $moves[[31]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[4]]$version_group
    ## $moves[[31]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[31]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[5]]
    ## $moves[[31]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[5]]$move_learn_method
    ## $moves[[31]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[5]]$version_group
    ## $moves[[31]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[31]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[6]]
    ## $moves[[31]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[6]]$move_learn_method
    ## $moves[[31]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[6]]$version_group
    ## $moves[[31]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[31]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[7]]
    ## $moves[[31]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[7]]$move_learn_method
    ## $moves[[31]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[7]]$version_group
    ## $moves[[31]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[31]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[8]]
    ## $moves[[31]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[8]]$move_learn_method
    ## $moves[[31]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[8]]$version_group
    ## $moves[[31]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[31]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[9]]
    ## $moves[[31]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[9]]$move_learn_method
    ## $moves[[31]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[9]]$version_group
    ## $moves[[31]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[31]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[10]]
    ## $moves[[31]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[10]]$move_learn_method
    ## $moves[[31]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[10]]$version_group
    ## $moves[[31]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[31]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[11]]
    ## $moves[[31]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[11]]$move_learn_method
    ## $moves[[31]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[11]]$version_group
    ## $moves[[31]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[31]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[12]]
    ## $moves[[31]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[12]]$move_learn_method
    ## $moves[[31]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[12]]$version_group
    ## $moves[[31]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[31]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[13]]
    ## $moves[[31]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[13]]$move_learn_method
    ## $moves[[31]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[13]]$version_group
    ## $moves[[31]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[31]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[14]]
    ## $moves[[31]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[31]]$version_group_details[[14]]$move_learn_method
    ## $moves[[31]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[31]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[31]]$version_group_details[[14]]$version_group
    ## $moves[[31]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[31]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[32]]
    ## $moves[[32]]$move
    ## $moves[[32]]$move$name
    ## [1] "amnesia"
    ## 
    ## $moves[[32]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/133/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details
    ## $moves[[32]]$version_group_details[[1]]
    ## $moves[[32]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[1]]$move_learn_method
    ## $moves[[32]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[1]]$version_group
    ## $moves[[32]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[32]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[2]]
    ## $moves[[32]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[2]]$move_learn_method
    ## $moves[[32]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[2]]$version_group
    ## $moves[[32]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[32]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[3]]
    ## $moves[[32]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[3]]$move_learn_method
    ## $moves[[32]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[3]]$version_group
    ## $moves[[32]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[32]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[4]]
    ## $moves[[32]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[4]]$move_learn_method
    ## $moves[[32]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[4]]$version_group
    ## $moves[[32]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[32]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[5]]
    ## $moves[[32]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[5]]$move_learn_method
    ## $moves[[32]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[5]]$version_group
    ## $moves[[32]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[32]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[6]]
    ## $moves[[32]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[6]]$move_learn_method
    ## $moves[[32]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[6]]$version_group
    ## $moves[[32]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[32]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[7]]
    ## $moves[[32]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[7]]$move_learn_method
    ## $moves[[32]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[7]]$version_group
    ## $moves[[32]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[32]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[8]]
    ## $moves[[32]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[8]]$move_learn_method
    ## $moves[[32]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[8]]$version_group
    ## $moves[[32]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[32]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[9]]
    ## $moves[[32]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[32]]$version_group_details[[9]]$move_learn_method
    ## $moves[[32]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[32]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[32]]$version_group_details[[9]]$version_group
    ## $moves[[32]]$version_group_details[[9]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[32]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[33]]
    ## $moves[[33]]$move
    ## $moves[[33]]$move$name
    ## [1] "flash"
    ## 
    ## $moves[[33]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/148/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details
    ## $moves[[33]]$version_group_details[[1]]
    ## $moves[[33]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[1]]$move_learn_method
    ## $moves[[33]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[1]]$version_group
    ## $moves[[33]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[33]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[2]]
    ## $moves[[33]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[2]]$move_learn_method
    ## $moves[[33]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[2]]$version_group
    ## $moves[[33]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[33]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[3]]
    ## $moves[[33]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[3]]$move_learn_method
    ## $moves[[33]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[3]]$version_group
    ## $moves[[33]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[33]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[4]]
    ## $moves[[33]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[4]]$move_learn_method
    ## $moves[[33]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[4]]$version_group
    ## $moves[[33]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[33]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[5]]
    ## $moves[[33]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[5]]$move_learn_method
    ## $moves[[33]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[5]]$version_group
    ## $moves[[33]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[33]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[6]]
    ## $moves[[33]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[6]]$move_learn_method
    ## $moves[[33]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[6]]$version_group
    ## $moves[[33]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[33]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[7]]
    ## $moves[[33]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[7]]$move_learn_method
    ## $moves[[33]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[7]]$version_group
    ## $moves[[33]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[33]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[8]]
    ## $moves[[33]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[8]]$move_learn_method
    ## $moves[[33]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[8]]$version_group
    ## $moves[[33]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[33]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[9]]
    ## $moves[[33]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[9]]$move_learn_method
    ## $moves[[33]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[9]]$version_group
    ## $moves[[33]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[33]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[10]]
    ## $moves[[33]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[10]]$move_learn_method
    ## $moves[[33]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[10]]$version_group
    ## $moves[[33]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[33]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[11]]
    ## $moves[[33]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[11]]$move_learn_method
    ## $moves[[33]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[11]]$version_group
    ## $moves[[33]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[33]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[12]]
    ## $moves[[33]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[12]]$move_learn_method
    ## $moves[[33]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[12]]$version_group
    ## $moves[[33]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[33]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[13]]
    ## $moves[[33]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[13]]$move_learn_method
    ## $moves[[33]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[13]]$version_group
    ## $moves[[33]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[33]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[14]]
    ## $moves[[33]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[33]]$version_group_details[[14]]$move_learn_method
    ## $moves[[33]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[33]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[33]]$version_group_details[[14]]$version_group
    ## $moves[[33]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[33]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[34]]
    ## $moves[[34]]$move
    ## $moves[[34]]$move$name
    ## [1] "rest"
    ## 
    ## $moves[[34]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/156/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details
    ## $moves[[34]]$version_group_details[[1]]
    ## $moves[[34]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[1]]$move_learn_method
    ## $moves[[34]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[1]]$version_group
    ## $moves[[34]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[34]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[2]]
    ## $moves[[34]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[2]]$move_learn_method
    ## $moves[[34]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[2]]$version_group
    ## $moves[[34]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[34]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[3]]
    ## $moves[[34]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[3]]$move_learn_method
    ## $moves[[34]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[3]]$version_group
    ## $moves[[34]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[34]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[4]]
    ## $moves[[34]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[4]]$move_learn_method
    ## $moves[[34]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[4]]$version_group
    ## $moves[[34]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[34]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[5]]
    ## $moves[[34]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[5]]$move_learn_method
    ## $moves[[34]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[5]]$version_group
    ## $moves[[34]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[34]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[6]]
    ## $moves[[34]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[6]]$move_learn_method
    ## $moves[[34]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[6]]$version_group
    ## $moves[[34]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[34]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[7]]
    ## $moves[[34]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[7]]$move_learn_method
    ## $moves[[34]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[7]]$version_group
    ## $moves[[34]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[34]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[8]]
    ## $moves[[34]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[8]]$move_learn_method
    ## $moves[[34]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[8]]$version_group
    ## $moves[[34]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[34]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[9]]
    ## $moves[[34]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[9]]$move_learn_method
    ## $moves[[34]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[9]]$version_group
    ## $moves[[34]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[34]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[10]]
    ## $moves[[34]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[10]]$move_learn_method
    ## $moves[[34]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[10]]$version_group
    ## $moves[[34]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[34]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[11]]
    ## $moves[[34]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[11]]$move_learn_method
    ## $moves[[34]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[11]]$version_group
    ## $moves[[34]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[34]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[12]]
    ## $moves[[34]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[12]]$move_learn_method
    ## $moves[[34]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[12]]$version_group
    ## $moves[[34]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[34]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[13]]
    ## $moves[[34]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[13]]$move_learn_method
    ## $moves[[34]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[13]]$version_group
    ## $moves[[34]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[34]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[14]]
    ## $moves[[34]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[14]]$move_learn_method
    ## $moves[[34]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[14]]$version_group
    ## $moves[[34]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[34]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[15]]
    ## $moves[[34]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[15]]$move_learn_method
    ## $moves[[34]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[15]]$version_group
    ## $moves[[34]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[34]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[16]]
    ## $moves[[34]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[16]]$move_learn_method
    ## $moves[[34]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[16]]$version_group
    ## $moves[[34]]$version_group_details[[16]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[34]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[17]]
    ## $moves[[34]]$version_group_details[[17]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[17]]$move_learn_method
    ## $moves[[34]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[17]]$version_group
    ## $moves[[34]]$version_group_details[[17]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[34]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[18]]
    ## $moves[[34]]$version_group_details[[18]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[34]]$version_group_details[[18]]$move_learn_method
    ## $moves[[34]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[34]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[34]]$version_group_details[[18]]$version_group
    ## $moves[[34]]$version_group_details[[18]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[34]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[35]]
    ## $moves[[35]]$move
    ## $moves[[35]]$move$name
    ## [1] "substitute"
    ## 
    ## $moves[[35]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/164/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details
    ## $moves[[35]]$version_group_details[[1]]
    ## $moves[[35]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[1]]$move_learn_method
    ## $moves[[35]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[1]]$version_group
    ## $moves[[35]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[35]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[2]]
    ## $moves[[35]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[2]]$move_learn_method
    ## $moves[[35]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[2]]$version_group
    ## $moves[[35]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[35]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[3]]
    ## $moves[[35]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[3]]$move_learn_method
    ## $moves[[35]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[3]]$version_group
    ## $moves[[35]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[35]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[4]]
    ## $moves[[35]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[4]]$move_learn_method
    ## $moves[[35]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[35]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[4]]$version_group
    ## $moves[[35]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[35]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[5]]
    ## $moves[[35]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[5]]$move_learn_method
    ## $moves[[35]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[5]]$version_group
    ## $moves[[35]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[35]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[6]]
    ## $moves[[35]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[6]]$move_learn_method
    ## $moves[[35]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[6]]$version_group
    ## $moves[[35]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[35]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[7]]
    ## $moves[[35]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[7]]$move_learn_method
    ## $moves[[35]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[7]]$version_group
    ## $moves[[35]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[35]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[8]]
    ## $moves[[35]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[8]]$move_learn_method
    ## $moves[[35]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[8]]$version_group
    ## $moves[[35]]$version_group_details[[8]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[35]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[9]]
    ## $moves[[35]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[9]]$move_learn_method
    ## $moves[[35]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[9]]$version_group
    ## $moves[[35]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[35]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[10]]
    ## $moves[[35]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[10]]$move_learn_method
    ## $moves[[35]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[35]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[10]]$version_group
    ## $moves[[35]]$version_group_details[[10]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[35]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[11]]
    ## $moves[[35]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[11]]$move_learn_method
    ## $moves[[35]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[35]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[11]]$version_group
    ## $moves[[35]]$version_group_details[[11]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[35]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[12]]
    ## $moves[[35]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[12]]$move_learn_method
    ## $moves[[35]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[12]]$version_group
    ## $moves[[35]]$version_group_details[[12]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[35]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[13]]
    ## $moves[[35]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[13]]$move_learn_method
    ## $moves[[35]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[13]]$version_group
    ## $moves[[35]]$version_group_details[[13]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[35]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[14]]
    ## $moves[[35]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[35]]$version_group_details[[14]]$move_learn_method
    ## $moves[[35]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[35]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[35]]$version_group_details[[14]]$version_group
    ## $moves[[35]]$version_group_details[[14]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[35]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[36]]
    ## $moves[[36]]$move
    ## $moves[[36]]$move$name
    ## [1] "snore"
    ## 
    ## $moves[[36]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/173/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details
    ## $moves[[36]]$version_group_details[[1]]
    ## $moves[[36]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[1]]$move_learn_method
    ## $moves[[36]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[36]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[1]]$version_group
    ## $moves[[36]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[36]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[2]]
    ## $moves[[36]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[2]]$move_learn_method
    ## $moves[[36]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[36]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[2]]$version_group
    ## $moves[[36]]$version_group_details[[2]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[36]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[3]]
    ## $moves[[36]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[3]]$move_learn_method
    ## $moves[[36]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[36]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[3]]$version_group
    ## $moves[[36]]$version_group_details[[3]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[36]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[4]]
    ## $moves[[36]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[4]]$move_learn_method
    ## $moves[[36]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[36]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[4]]$version_group
    ## $moves[[36]]$version_group_details[[4]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[36]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[5]]
    ## $moves[[36]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[5]]$move_learn_method
    ## $moves[[36]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[36]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[5]]$version_group
    ## $moves[[36]]$version_group_details[[5]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[36]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[6]]
    ## $moves[[36]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[6]]$move_learn_method
    ## $moves[[36]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[36]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[6]]$version_group
    ## $moves[[36]]$version_group_details[[6]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[36]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[7]]
    ## $moves[[36]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[36]]$version_group_details[[7]]$move_learn_method
    ## $moves[[36]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[36]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[36]]$version_group_details[[7]]$version_group
    ## $moves[[36]]$version_group_details[[7]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[36]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[37]]
    ## $moves[[37]]$move
    ## $moves[[37]]$move$name
    ## [1] "curse"
    ## 
    ## $moves[[37]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/174/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details
    ## $moves[[37]]$version_group_details[[1]]
    ## $moves[[37]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[1]]$move_learn_method
    ## $moves[[37]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[1]]$version_group
    ## $moves[[37]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[37]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[2]]
    ## $moves[[37]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[2]]$move_learn_method
    ## $moves[[37]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[2]]$version_group
    ## $moves[[37]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[37]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[3]]
    ## $moves[[37]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[3]]$move_learn_method
    ## $moves[[37]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[3]]$version_group
    ## $moves[[37]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[37]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[4]]
    ## $moves[[37]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[4]]$move_learn_method
    ## $moves[[37]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[4]]$version_group
    ## $moves[[37]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[37]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[5]]
    ## $moves[[37]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[5]]$move_learn_method
    ## $moves[[37]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[5]]$version_group
    ## $moves[[37]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[37]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[6]]
    ## $moves[[37]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[6]]$move_learn_method
    ## $moves[[37]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[6]]$version_group
    ## $moves[[37]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[37]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[7]]
    ## $moves[[37]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[7]]$move_learn_method
    ## $moves[[37]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[7]]$version_group
    ## $moves[[37]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[37]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[8]]
    ## $moves[[37]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[8]]$move_learn_method
    ## $moves[[37]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[8]]$version_group
    ## $moves[[37]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[37]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[9]]
    ## $moves[[37]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[9]]$move_learn_method
    ## $moves[[37]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[9]]$version_group
    ## $moves[[37]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[37]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[10]]
    ## $moves[[37]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[10]]$move_learn_method
    ## $moves[[37]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[10]]$version_group
    ## $moves[[37]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[37]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[11]]
    ## $moves[[37]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[11]]$move_learn_method
    ## $moves[[37]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[11]]$version_group
    ## $moves[[37]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[37]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[12]]
    ## $moves[[37]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[12]]$move_learn_method
    ## $moves[[37]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[37]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[12]]$version_group
    ## $moves[[37]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[37]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[13]]
    ## $moves[[37]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[13]]$move_learn_method
    ## $moves[[37]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[37]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[13]]$version_group
    ## $moves[[37]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[37]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[14]]
    ## $moves[[37]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[37]]$version_group_details[[14]]$move_learn_method
    ## $moves[[37]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[37]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[37]]$version_group_details[[14]]$version_group
    ## $moves[[37]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[37]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[38]]
    ## $moves[[38]]$move
    ## $moves[[38]]$move$name
    ## [1] "protect"
    ## 
    ## $moves[[38]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/182/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details
    ## $moves[[38]]$version_group_details[[1]]
    ## $moves[[38]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[1]]$move_learn_method
    ## $moves[[38]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[1]]$version_group
    ## $moves[[38]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[38]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[2]]
    ## $moves[[38]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[2]]$move_learn_method
    ## $moves[[38]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[2]]$version_group
    ## $moves[[38]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[38]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[3]]
    ## $moves[[38]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[3]]$move_learn_method
    ## $moves[[38]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[3]]$version_group
    ## $moves[[38]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[38]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[4]]
    ## $moves[[38]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[4]]$move_learn_method
    ## $moves[[38]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[4]]$version_group
    ## $moves[[38]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[38]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[5]]
    ## $moves[[38]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[5]]$move_learn_method
    ## $moves[[38]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[5]]$version_group
    ## $moves[[38]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[38]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[6]]
    ## $moves[[38]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[6]]$move_learn_method
    ## $moves[[38]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[6]]$version_group
    ## $moves[[38]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[38]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[7]]
    ## $moves[[38]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[7]]$move_learn_method
    ## $moves[[38]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[7]]$version_group
    ## $moves[[38]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[38]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[8]]
    ## $moves[[38]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[8]]$move_learn_method
    ## $moves[[38]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[8]]$version_group
    ## $moves[[38]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[38]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[9]]
    ## $moves[[38]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[9]]$move_learn_method
    ## $moves[[38]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[9]]$version_group
    ## $moves[[38]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[38]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[10]]
    ## $moves[[38]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[10]]$move_learn_method
    ## $moves[[38]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[10]]$version_group
    ## $moves[[38]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[38]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[11]]
    ## $moves[[38]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[11]]$move_learn_method
    ## $moves[[38]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[11]]$version_group
    ## $moves[[38]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[38]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[12]]
    ## $moves[[38]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[12]]$move_learn_method
    ## $moves[[38]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[12]]$version_group
    ## $moves[[38]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[38]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[13]]
    ## $moves[[38]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[13]]$move_learn_method
    ## $moves[[38]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[13]]$version_group
    ## $moves[[38]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[38]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[14]]
    ## $moves[[38]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[14]]$move_learn_method
    ## $moves[[38]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[14]]$version_group
    ## $moves[[38]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[38]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[15]]
    ## $moves[[38]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[15]]$move_learn_method
    ## $moves[[38]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[15]]$version_group
    ## $moves[[38]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[38]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[16]]
    ## $moves[[38]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[38]]$version_group_details[[16]]$move_learn_method
    ## $moves[[38]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[38]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[38]]$version_group_details[[16]]$version_group
    ## $moves[[38]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[38]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[39]]
    ## $moves[[39]]$move
    ## $moves[[39]]$move$name
    ## [1] "sludge-bomb"
    ## 
    ## $moves[[39]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/188/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details
    ## $moves[[39]]$version_group_details[[1]]
    ## $moves[[39]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[1]]$move_learn_method
    ## $moves[[39]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[1]]$version_group
    ## $moves[[39]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[39]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[2]]
    ## $moves[[39]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[2]]$move_learn_method
    ## $moves[[39]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[2]]$version_group
    ## $moves[[39]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[39]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[3]]
    ## $moves[[39]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[3]]$move_learn_method
    ## $moves[[39]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[3]]$version_group
    ## $moves[[39]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[39]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[4]]
    ## $moves[[39]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[4]]$move_learn_method
    ## $moves[[39]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[4]]$version_group
    ## $moves[[39]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[39]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[5]]
    ## $moves[[39]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[5]]$move_learn_method
    ## $moves[[39]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[5]]$version_group
    ## $moves[[39]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[39]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[6]]
    ## $moves[[39]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[6]]$move_learn_method
    ## $moves[[39]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[6]]$version_group
    ## $moves[[39]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[39]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[7]]
    ## $moves[[39]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[7]]$move_learn_method
    ## $moves[[39]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[7]]$version_group
    ## $moves[[39]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[39]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[8]]
    ## $moves[[39]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[8]]$move_learn_method
    ## $moves[[39]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[8]]$version_group
    ## $moves[[39]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[39]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[9]]
    ## $moves[[39]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[9]]$move_learn_method
    ## $moves[[39]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[9]]$version_group
    ## $moves[[39]]$version_group_details[[9]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[39]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[10]]
    ## $moves[[39]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[10]]$move_learn_method
    ## $moves[[39]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[10]]$version_group
    ## $moves[[39]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[39]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[11]]
    ## $moves[[39]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[11]]$move_learn_method
    ## $moves[[39]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[11]]$version_group
    ## $moves[[39]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[39]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[12]]
    ## $moves[[39]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[12]]$move_learn_method
    ## $moves[[39]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[12]]$version_group
    ## $moves[[39]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[39]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[13]]
    ## $moves[[39]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[13]]$move_learn_method
    ## $moves[[39]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[13]]$version_group
    ## $moves[[39]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[39]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[14]]
    ## $moves[[39]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[39]]$version_group_details[[14]]$move_learn_method
    ## $moves[[39]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[39]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[39]]$version_group_details[[14]]$version_group
    ## $moves[[39]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[39]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[40]]
    ## $moves[[40]]$move
    ## $moves[[40]]$move$name
    ## [1] "mud-slap"
    ## 
    ## $moves[[40]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/189/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details
    ## $moves[[40]]$version_group_details[[1]]
    ## $moves[[40]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[40]]$version_group_details[[1]]$move_learn_method
    ## $moves[[40]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[40]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[1]]$version_group
    ## $moves[[40]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[40]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[2]]
    ## $moves[[40]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[40]]$version_group_details[[2]]$move_learn_method
    ## $moves[[40]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[40]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[2]]$version_group
    ## $moves[[40]]$version_group_details[[2]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[40]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[3]]
    ## $moves[[40]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[40]]$version_group_details[[3]]$move_learn_method
    ## $moves[[40]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[40]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[3]]$version_group
    ## $moves[[40]]$version_group_details[[3]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[40]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[4]]
    ## $moves[[40]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[40]]$version_group_details[[4]]$move_learn_method
    ## $moves[[40]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[40]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[4]]$version_group
    ## $moves[[40]]$version_group_details[[4]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[40]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[5]]
    ## $moves[[40]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[40]]$version_group_details[[5]]$move_learn_method
    ## $moves[[40]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[40]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[40]]$version_group_details[[5]]$version_group
    ## $moves[[40]]$version_group_details[[5]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[40]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[41]]
    ## $moves[[41]]$move
    ## $moves[[41]]$move$name
    ## [1] "giga-drain"
    ## 
    ## $moves[[41]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/202/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details
    ## $moves[[41]]$version_group_details[[1]]
    ## $moves[[41]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[1]]$move_learn_method
    ## $moves[[41]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[41]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[1]]$version_group
    ## $moves[[41]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[41]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[2]]
    ## $moves[[41]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[2]]$move_learn_method
    ## $moves[[41]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[2]]$version_group
    ## $moves[[41]]$version_group_details[[2]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[41]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[3]]
    ## $moves[[41]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[3]]$move_learn_method
    ## $moves[[41]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[3]]$version_group
    ## $moves[[41]]$version_group_details[[3]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[41]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[4]]
    ## $moves[[41]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[4]]$move_learn_method
    ## $moves[[41]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[41]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[4]]$version_group
    ## $moves[[41]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[41]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[5]]
    ## $moves[[41]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[5]]$move_learn_method
    ## $moves[[41]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[5]]$version_group
    ## $moves[[41]]$version_group_details[[5]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[41]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[6]]
    ## $moves[[41]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[6]]$move_learn_method
    ## $moves[[41]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[6]]$version_group
    ## $moves[[41]]$version_group_details[[6]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[41]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[7]]
    ## $moves[[41]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[7]]$move_learn_method
    ## $moves[[41]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[7]]$version_group
    ## $moves[[41]]$version_group_details[[7]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[41]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[8]]
    ## $moves[[41]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[8]]$move_learn_method
    ## $moves[[41]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[8]]$version_group
    ## $moves[[41]]$version_group_details[[8]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[41]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[9]]
    ## $moves[[41]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[9]]$move_learn_method
    ## $moves[[41]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[9]]$version_group
    ## $moves[[41]]$version_group_details[[9]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[41]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[10]]
    ## $moves[[41]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[10]]$move_learn_method
    ## $moves[[41]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[10]]$version_group
    ## $moves[[41]]$version_group_details[[10]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[41]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[11]]
    ## $moves[[41]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[11]]$move_learn_method
    ## $moves[[41]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[11]]$version_group
    ## $moves[[41]]$version_group_details[[11]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[41]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[12]]
    ## $moves[[41]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[12]]$move_learn_method
    ## $moves[[41]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[12]]$version_group
    ## $moves[[41]]$version_group_details[[12]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[41]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[13]]
    ## $moves[[41]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[13]]$move_learn_method
    ## $moves[[41]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[13]]$version_group
    ## $moves[[41]]$version_group_details[[13]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[41]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[14]]
    ## $moves[[41]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[14]]$move_learn_method
    ## $moves[[41]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[14]]$version_group
    ## $moves[[41]]$version_group_details[[14]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[41]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[15]]
    ## $moves[[41]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[15]]$move_learn_method
    ## $moves[[41]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[15]]$version_group
    ## $moves[[41]]$version_group_details[[15]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[41]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[16]]
    ## $moves[[41]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[16]]$move_learn_method
    ## $moves[[41]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[16]]$version_group
    ## $moves[[41]]$version_group_details[[16]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[41]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[17]]
    ## $moves[[41]]$version_group_details[[17]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[17]]$move_learn_method
    ## $moves[[41]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[41]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[17]]$version_group
    ## $moves[[41]]$version_group_details[[17]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[41]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[18]]
    ## $moves[[41]]$version_group_details[[18]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[41]]$version_group_details[[18]]$move_learn_method
    ## $moves[[41]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[41]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[41]]$version_group_details[[18]]$version_group
    ## $moves[[41]]$version_group_details[[18]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[41]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[42]]
    ## $moves[[42]]$move
    ## $moves[[42]]$move$name
    ## [1] "endure"
    ## 
    ## $moves[[42]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/203/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details
    ## $moves[[42]]$version_group_details[[1]]
    ## $moves[[42]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[1]]$move_learn_method
    ## $moves[[42]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[1]]$version_group
    ## $moves[[42]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[42]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[2]]
    ## $moves[[42]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[2]]$move_learn_method
    ## $moves[[42]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[2]]$version_group
    ## $moves[[42]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[42]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[3]]
    ## $moves[[42]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[3]]$move_learn_method
    ## $moves[[42]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[3]]$version_group
    ## $moves[[42]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[42]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[4]]
    ## $moves[[42]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[4]]$move_learn_method
    ## $moves[[42]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[42]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[4]]$version_group
    ## $moves[[42]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[42]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[5]]
    ## $moves[[42]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[5]]$move_learn_method
    ## $moves[[42]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[42]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[5]]$version_group
    ## $moves[[42]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[42]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[6]]
    ## $moves[[42]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[6]]$move_learn_method
    ## $moves[[42]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[42]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[6]]$version_group
    ## $moves[[42]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[42]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[7]]
    ## $moves[[42]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[7]]$move_learn_method
    ## $moves[[42]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[7]]$version_group
    ## $moves[[42]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[42]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[8]]
    ## $moves[[42]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[8]]$move_learn_method
    ## $moves[[42]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[8]]$version_group
    ## $moves[[42]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[42]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[9]]
    ## $moves[[42]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[9]]$move_learn_method
    ## $moves[[42]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[42]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[9]]$version_group
    ## $moves[[42]]$version_group_details[[9]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[42]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[10]]
    ## $moves[[42]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[10]]$move_learn_method
    ## $moves[[42]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[42]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[10]]$version_group
    ## $moves[[42]]$version_group_details[[10]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[42]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[11]]
    ## $moves[[42]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[11]]$move_learn_method
    ## $moves[[42]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[42]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[11]]$version_group
    ## $moves[[42]]$version_group_details[[11]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[42]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[12]]
    ## $moves[[42]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[42]]$version_group_details[[12]]$move_learn_method
    ## $moves[[42]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[42]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[42]]$version_group_details[[12]]$version_group
    ## $moves[[42]]$version_group_details[[12]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[42]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[43]]
    ## $moves[[43]]$move
    ## $moves[[43]]$move$name
    ## [1] "charm"
    ## 
    ## $moves[[43]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/204/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details
    ## $moves[[43]]$version_group_details[[1]]
    ## $moves[[43]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[1]]$move_learn_method
    ## $moves[[43]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[1]]$version_group
    ## $moves[[43]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[43]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[2]]
    ## $moves[[43]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[2]]$move_learn_method
    ## $moves[[43]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[2]]$version_group
    ## $moves[[43]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[43]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[3]]
    ## $moves[[43]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[3]]$move_learn_method
    ## $moves[[43]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[3]]$version_group
    ## $moves[[43]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[43]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[4]]
    ## $moves[[43]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[4]]$move_learn_method
    ## $moves[[43]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[4]]$version_group
    ## $moves[[43]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[43]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[5]]
    ## $moves[[43]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[5]]$move_learn_method
    ## $moves[[43]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[5]]$version_group
    ## $moves[[43]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[43]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[6]]
    ## $moves[[43]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[6]]$move_learn_method
    ## $moves[[43]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[6]]$version_group
    ## $moves[[43]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[43]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[7]]
    ## $moves[[43]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[7]]$move_learn_method
    ## $moves[[43]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[7]]$version_group
    ## $moves[[43]]$version_group_details[[7]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[43]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[8]]
    ## $moves[[43]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[8]]$move_learn_method
    ## $moves[[43]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[8]]$version_group
    ## $moves[[43]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[43]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[9]]
    ## $moves[[43]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[9]]$move_learn_method
    ## $moves[[43]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[9]]$version_group
    ## $moves[[43]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[43]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[10]]
    ## $moves[[43]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[10]]$move_learn_method
    ## $moves[[43]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[10]]$version_group
    ## $moves[[43]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[43]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[11]]
    ## $moves[[43]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[11]]$move_learn_method
    ## $moves[[43]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[11]]$version_group
    ## $moves[[43]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[43]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[12]]
    ## $moves[[43]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[12]]$move_learn_method
    ## $moves[[43]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[12]]$version_group
    ## $moves[[43]]$version_group_details[[12]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[43]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[13]]
    ## $moves[[43]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[43]]$version_group_details[[13]]$move_learn_method
    ## $moves[[43]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[43]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[43]]$version_group_details[[13]]$version_group
    ## $moves[[43]]$version_group_details[[13]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[43]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[44]]
    ## $moves[[44]]$move
    ## $moves[[44]]$move$name
    ## [1] "swagger"
    ## 
    ## $moves[[44]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/207/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details
    ## $moves[[44]]$version_group_details[[1]]
    ## $moves[[44]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[1]]$move_learn_method
    ## $moves[[44]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[1]]$version_group
    ## $moves[[44]]$version_group_details[[1]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[44]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[2]]
    ## $moves[[44]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[2]]$move_learn_method
    ## $moves[[44]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[2]]$version_group
    ## $moves[[44]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[44]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[3]]
    ## $moves[[44]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[3]]$move_learn_method
    ## $moves[[44]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[3]]$version_group
    ## $moves[[44]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[44]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[4]]
    ## $moves[[44]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[4]]$move_learn_method
    ## $moves[[44]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[44]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[4]]$version_group
    ## $moves[[44]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[44]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[5]]
    ## $moves[[44]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[5]]$move_learn_method
    ## $moves[[44]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[5]]$version_group
    ## $moves[[44]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[44]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[6]]
    ## $moves[[44]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[6]]$move_learn_method
    ## $moves[[44]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[6]]$version_group
    ## $moves[[44]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[44]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[7]]
    ## $moves[[44]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[7]]$move_learn_method
    ## $moves[[44]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[7]]$version_group
    ## $moves[[44]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[44]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[8]]
    ## $moves[[44]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[8]]$move_learn_method
    ## $moves[[44]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[8]]$version_group
    ## $moves[[44]]$version_group_details[[8]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[44]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[9]]
    ## $moves[[44]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[9]]$move_learn_method
    ## $moves[[44]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[9]]$version_group
    ## $moves[[44]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[44]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[10]]
    ## $moves[[44]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[10]]$move_learn_method
    ## $moves[[44]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[44]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[10]]$version_group
    ## $moves[[44]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[44]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[11]]
    ## $moves[[44]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[11]]$move_learn_method
    ## $moves[[44]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[11]]$version_group
    ## $moves[[44]]$version_group_details[[11]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[44]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[12]]
    ## $moves[[44]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[12]]$move_learn_method
    ## $moves[[44]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[12]]$version_group
    ## $moves[[44]]$version_group_details[[12]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[44]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[13]]
    ## $moves[[44]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[44]]$version_group_details[[13]]$move_learn_method
    ## $moves[[44]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[44]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[44]]$version_group_details[[13]]$version_group
    ## $moves[[44]]$version_group_details[[13]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[44]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[45]]
    ## $moves[[45]]$move
    ## $moves[[45]]$move$name
    ## [1] "fury-cutter"
    ## 
    ## $moves[[45]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/210/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details
    ## $moves[[45]]$version_group_details[[1]]
    ## $moves[[45]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[45]]$version_group_details[[1]]$move_learn_method
    ## $moves[[45]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[45]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[1]]$version_group
    ## $moves[[45]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[45]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[2]]
    ## $moves[[45]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[45]]$version_group_details[[2]]$move_learn_method
    ## $moves[[45]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[45]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[2]]$version_group
    ## $moves[[45]]$version_group_details[[2]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[45]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[3]]
    ## $moves[[45]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[45]]$version_group_details[[3]]$move_learn_method
    ## $moves[[45]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[45]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[3]]$version_group
    ## $moves[[45]]$version_group_details[[3]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[45]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[4]]
    ## $moves[[45]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[45]]$version_group_details[[4]]$move_learn_method
    ## $moves[[45]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[45]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[4]]$version_group
    ## $moves[[45]]$version_group_details[[4]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[45]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[5]]
    ## $moves[[45]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[45]]$version_group_details[[5]]$move_learn_method
    ## $moves[[45]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[45]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[45]]$version_group_details[[5]]$version_group
    ## $moves[[45]]$version_group_details[[5]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[45]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[46]]
    ## $moves[[46]]$move
    ## $moves[[46]]$move$name
    ## [1] "attract"
    ## 
    ## $moves[[46]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/213/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details
    ## $moves[[46]]$version_group_details[[1]]
    ## $moves[[46]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[1]]$move_learn_method
    ## $moves[[46]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[1]]$version_group
    ## $moves[[46]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[46]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[2]]
    ## $moves[[46]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[2]]$move_learn_method
    ## $moves[[46]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[2]]$version_group
    ## $moves[[46]]$version_group_details[[2]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[46]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[3]]
    ## $moves[[46]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[3]]$move_learn_method
    ## $moves[[46]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[3]]$version_group
    ## $moves[[46]]$version_group_details[[3]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[46]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[4]]
    ## $moves[[46]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[4]]$move_learn_method
    ## $moves[[46]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[4]]$version_group
    ## $moves[[46]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[46]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[5]]
    ## $moves[[46]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[5]]$move_learn_method
    ## $moves[[46]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[5]]$version_group
    ## $moves[[46]]$version_group_details[[5]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[46]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[6]]
    ## $moves[[46]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[6]]$move_learn_method
    ## $moves[[46]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[6]]$version_group
    ## $moves[[46]]$version_group_details[[6]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[46]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[7]]
    ## $moves[[46]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[7]]$move_learn_method
    ## $moves[[46]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[7]]$version_group
    ## $moves[[46]]$version_group_details[[7]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[46]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[8]]
    ## $moves[[46]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[8]]$move_learn_method
    ## $moves[[46]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[8]]$version_group
    ## $moves[[46]]$version_group_details[[8]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[46]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[9]]
    ## $moves[[46]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[9]]$move_learn_method
    ## $moves[[46]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[9]]$version_group
    ## $moves[[46]]$version_group_details[[9]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[46]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[10]]
    ## $moves[[46]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[10]]$move_learn_method
    ## $moves[[46]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[10]]$version_group
    ## $moves[[46]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[46]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[11]]
    ## $moves[[46]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[11]]$move_learn_method
    ## $moves[[46]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[11]]$version_group
    ## $moves[[46]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[46]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[12]]
    ## $moves[[46]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[12]]$move_learn_method
    ## $moves[[46]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[12]]$version_group
    ## $moves[[46]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[46]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[13]]
    ## $moves[[46]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[13]]$move_learn_method
    ## $moves[[46]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[13]]$version_group
    ## $moves[[46]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[46]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[14]]
    ## $moves[[46]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[14]]$move_learn_method
    ## $moves[[46]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[14]]$version_group
    ## $moves[[46]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[46]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[15]]
    ## $moves[[46]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[15]]$move_learn_method
    ## $moves[[46]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[15]]$version_group
    ## $moves[[46]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[46]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[16]]
    ## $moves[[46]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[46]]$version_group_details[[16]]$move_learn_method
    ## $moves[[46]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[46]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[46]]$version_group_details[[16]]$version_group
    ## $moves[[46]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[46]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[47]]
    ## $moves[[47]]$move
    ## $moves[[47]]$move$name
    ## [1] "sleep-talk"
    ## 
    ## $moves[[47]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/214/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details
    ## $moves[[47]]$version_group_details[[1]]
    ## $moves[[47]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[1]]$move_learn_method
    ## $moves[[47]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[1]]$version_group
    ## $moves[[47]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[47]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[2]]
    ## $moves[[47]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[2]]$move_learn_method
    ## $moves[[47]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[2]]$version_group
    ## $moves[[47]]$version_group_details[[2]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[47]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[3]]
    ## $moves[[47]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[3]]$move_learn_method
    ## $moves[[47]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[3]]$version_group
    ## $moves[[47]]$version_group_details[[3]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[47]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[4]]
    ## $moves[[47]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[4]]$move_learn_method
    ## $moves[[47]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[47]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[4]]$version_group
    ## $moves[[47]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[47]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[5]]
    ## $moves[[47]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[5]]$move_learn_method
    ## $moves[[47]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[5]]$version_group
    ## $moves[[47]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[47]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[6]]
    ## $moves[[47]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[6]]$move_learn_method
    ## $moves[[47]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[6]]$version_group
    ## $moves[[47]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[47]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[7]]
    ## $moves[[47]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[7]]$move_learn_method
    ## $moves[[47]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[7]]$version_group
    ## $moves[[47]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[47]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[8]]
    ## $moves[[47]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[8]]$move_learn_method
    ## $moves[[47]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[8]]$version_group
    ## $moves[[47]]$version_group_details[[8]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[47]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[9]]
    ## $moves[[47]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[9]]$move_learn_method
    ## $moves[[47]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[47]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[9]]$version_group
    ## $moves[[47]]$version_group_details[[9]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[47]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[10]]
    ## $moves[[47]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[10]]$move_learn_method
    ## $moves[[47]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[10]]$version_group
    ## $moves[[47]]$version_group_details[[10]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[47]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[11]]
    ## $moves[[47]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[47]]$version_group_details[[11]]$move_learn_method
    ## $moves[[47]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[47]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[47]]$version_group_details[[11]]$version_group
    ## $moves[[47]]$version_group_details[[11]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[47]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[48]]
    ## $moves[[48]]$move
    ## $moves[[48]]$move$name
    ## [1] "return"
    ## 
    ## $moves[[48]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/216/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details
    ## $moves[[48]]$version_group_details[[1]]
    ## $moves[[48]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[1]]$move_learn_method
    ## $moves[[48]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[1]]$version_group
    ## $moves[[48]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[48]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[2]]
    ## $moves[[48]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[2]]$move_learn_method
    ## $moves[[48]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[2]]$version_group
    ## $moves[[48]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[48]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[3]]
    ## $moves[[48]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[3]]$move_learn_method
    ## $moves[[48]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[3]]$version_group
    ## $moves[[48]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[48]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[4]]
    ## $moves[[48]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[4]]$move_learn_method
    ## $moves[[48]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[4]]$version_group
    ## $moves[[48]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[48]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[5]]
    ## $moves[[48]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[5]]$move_learn_method
    ## $moves[[48]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[5]]$version_group
    ## $moves[[48]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[48]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[6]]
    ## $moves[[48]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[6]]$move_learn_method
    ## $moves[[48]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[6]]$version_group
    ## $moves[[48]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[48]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[7]]
    ## $moves[[48]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[7]]$move_learn_method
    ## $moves[[48]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[7]]$version_group
    ## $moves[[48]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[48]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[8]]
    ## $moves[[48]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[8]]$move_learn_method
    ## $moves[[48]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[8]]$version_group
    ## $moves[[48]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[48]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[9]]
    ## $moves[[48]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[9]]$move_learn_method
    ## $moves[[48]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[9]]$version_group
    ## $moves[[48]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[48]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[10]]
    ## $moves[[48]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[10]]$move_learn_method
    ## $moves[[48]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[10]]$version_group
    ## $moves[[48]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[48]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[11]]
    ## $moves[[48]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[11]]$move_learn_method
    ## $moves[[48]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[11]]$version_group
    ## $moves[[48]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[48]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[12]]
    ## $moves[[48]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[12]]$move_learn_method
    ## $moves[[48]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[12]]$version_group
    ## $moves[[48]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[48]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[13]]
    ## $moves[[48]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[13]]$move_learn_method
    ## $moves[[48]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[13]]$version_group
    ## $moves[[48]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[48]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[14]]
    ## $moves[[48]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[14]]$move_learn_method
    ## $moves[[48]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[14]]$version_group
    ## $moves[[48]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[48]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[15]]
    ## $moves[[48]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[15]]$move_learn_method
    ## $moves[[48]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[15]]$version_group
    ## $moves[[48]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[48]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[16]]
    ## $moves[[48]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[48]]$version_group_details[[16]]$move_learn_method
    ## $moves[[48]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[48]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[48]]$version_group_details[[16]]$version_group
    ## $moves[[48]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[48]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[49]]
    ## $moves[[49]]$move
    ## $moves[[49]]$move$name
    ## [1] "frustration"
    ## 
    ## $moves[[49]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/218/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details
    ## $moves[[49]]$version_group_details[[1]]
    ## $moves[[49]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[1]]$move_learn_method
    ## $moves[[49]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[1]]$version_group
    ## $moves[[49]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[49]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[2]]
    ## $moves[[49]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[2]]$move_learn_method
    ## $moves[[49]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[2]]$version_group
    ## $moves[[49]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[49]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[3]]
    ## $moves[[49]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[3]]$move_learn_method
    ## $moves[[49]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[3]]$version_group
    ## $moves[[49]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[49]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[4]]
    ## $moves[[49]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[4]]$move_learn_method
    ## $moves[[49]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[4]]$version_group
    ## $moves[[49]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[49]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[5]]
    ## $moves[[49]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[5]]$move_learn_method
    ## $moves[[49]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[5]]$version_group
    ## $moves[[49]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[49]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[6]]
    ## $moves[[49]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[6]]$move_learn_method
    ## $moves[[49]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[6]]$version_group
    ## $moves[[49]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[49]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[7]]
    ## $moves[[49]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[7]]$move_learn_method
    ## $moves[[49]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[7]]$version_group
    ## $moves[[49]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[49]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[8]]
    ## $moves[[49]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[8]]$move_learn_method
    ## $moves[[49]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[8]]$version_group
    ## $moves[[49]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[49]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[9]]
    ## $moves[[49]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[9]]$move_learn_method
    ## $moves[[49]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[9]]$version_group
    ## $moves[[49]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[49]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[10]]
    ## $moves[[49]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[10]]$move_learn_method
    ## $moves[[49]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[10]]$version_group
    ## $moves[[49]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[49]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[11]]
    ## $moves[[49]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[11]]$move_learn_method
    ## $moves[[49]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[11]]$version_group
    ## $moves[[49]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[49]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[12]]
    ## $moves[[49]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[12]]$move_learn_method
    ## $moves[[49]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[12]]$version_group
    ## $moves[[49]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[49]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[13]]
    ## $moves[[49]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[13]]$move_learn_method
    ## $moves[[49]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[13]]$version_group
    ## $moves[[49]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[49]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[14]]
    ## $moves[[49]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[14]]$move_learn_method
    ## $moves[[49]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[14]]$version_group
    ## $moves[[49]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[49]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[15]]
    ## $moves[[49]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[15]]$move_learn_method
    ## $moves[[49]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[15]]$version_group
    ## $moves[[49]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[49]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[16]]
    ## $moves[[49]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[49]]$version_group_details[[16]]$move_learn_method
    ## $moves[[49]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[49]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[49]]$version_group_details[[16]]$version_group
    ## $moves[[49]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[49]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[50]]
    ## $moves[[50]]$move
    ## $moves[[50]]$move$name
    ## [1] "safeguard"
    ## 
    ## $moves[[50]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/219/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details
    ## $moves[[50]]$version_group_details[[1]]
    ## $moves[[50]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[1]]$move_learn_method
    ## $moves[[50]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[1]]$version_group
    ## $moves[[50]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[50]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[2]]
    ## $moves[[50]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[2]]$move_learn_method
    ## $moves[[50]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[2]]$version_group
    ## $moves[[50]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[50]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[3]]
    ## $moves[[50]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[3]]$move_learn_method
    ## $moves[[50]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[3]]$version_group
    ## $moves[[50]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[50]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[4]]
    ## $moves[[50]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[4]]$move_learn_method
    ## $moves[[50]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[4]]$version_group
    ## $moves[[50]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[50]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[5]]
    ## $moves[[50]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[5]]$move_learn_method
    ## $moves[[50]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[5]]$version_group
    ## $moves[[50]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[50]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[6]]
    ## $moves[[50]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[6]]$move_learn_method
    ## $moves[[50]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[6]]$version_group
    ## $moves[[50]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[50]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[7]]
    ## $moves[[50]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[7]]$move_learn_method
    ## $moves[[50]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[7]]$version_group
    ## $moves[[50]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[50]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[8]]
    ## $moves[[50]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[8]]$move_learn_method
    ## $moves[[50]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[8]]$version_group
    ## $moves[[50]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[50]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[9]]
    ## $moves[[50]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[9]]$move_learn_method
    ## $moves[[50]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[9]]$version_group
    ## $moves[[50]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[50]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[10]]
    ## $moves[[50]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[10]]$move_learn_method
    ## $moves[[50]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[10]]$version_group
    ## $moves[[50]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[50]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[11]]
    ## $moves[[50]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[11]]$move_learn_method
    ## $moves[[50]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[11]]$version_group
    ## $moves[[50]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[50]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[12]]
    ## $moves[[50]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[12]]$move_learn_method
    ## $moves[[50]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[12]]$version_group
    ## $moves[[50]]$version_group_details[[12]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[50]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[13]]
    ## $moves[[50]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[13]]$move_learn_method
    ## $moves[[50]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[50]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[13]]$version_group
    ## $moves[[50]]$version_group_details[[13]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[50]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[14]]
    ## $moves[[50]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[50]]$version_group_details[[14]]$move_learn_method
    ## $moves[[50]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[50]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[50]]$version_group_details[[14]]$version_group
    ## $moves[[50]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[50]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[51]]
    ## $moves[[51]]$move
    ## $moves[[51]]$move$name
    ## [1] "sweet-scent"
    ## 
    ## $moves[[51]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/230/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details
    ## $moves[[51]]$version_group_details[[1]]
    ## $moves[[51]]$version_group_details[[1]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[1]]$move_learn_method
    ## $moves[[51]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[1]]$version_group
    ## $moves[[51]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[51]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[2]]
    ## $moves[[51]]$version_group_details[[2]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[2]]$move_learn_method
    ## $moves[[51]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[2]]$version_group
    ## $moves[[51]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[51]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[3]]
    ## $moves[[51]]$version_group_details[[3]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[3]]$move_learn_method
    ## $moves[[51]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[3]]$version_group
    ## $moves[[51]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[51]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[4]]
    ## $moves[[51]]$version_group_details[[4]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[4]]$move_learn_method
    ## $moves[[51]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[4]]$version_group
    ## $moves[[51]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[51]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[5]]
    ## $moves[[51]]$version_group_details[[5]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[5]]$move_learn_method
    ## $moves[[51]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[5]]$version_group
    ## $moves[[51]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[51]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[6]]
    ## $moves[[51]]$version_group_details[[6]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[6]]$move_learn_method
    ## $moves[[51]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[6]]$version_group
    ## $moves[[51]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[51]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[7]]
    ## $moves[[51]]$version_group_details[[7]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[7]]$move_learn_method
    ## $moves[[51]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[7]]$version_group
    ## $moves[[51]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[51]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[8]]
    ## $moves[[51]]$version_group_details[[8]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[8]]$move_learn_method
    ## $moves[[51]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[8]]$version_group
    ## $moves[[51]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[51]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[9]]
    ## $moves[[51]]$version_group_details[[9]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[9]]$move_learn_method
    ## $moves[[51]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[9]]$version_group
    ## $moves[[51]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[51]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[10]]
    ## $moves[[51]]$version_group_details[[10]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[10]]$move_learn_method
    ## $moves[[51]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[10]]$version_group
    ## $moves[[51]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[51]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[11]]
    ## $moves[[51]]$version_group_details[[11]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[11]]$move_learn_method
    ## $moves[[51]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[11]]$version_group
    ## $moves[[51]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[51]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[12]]
    ## $moves[[51]]$version_group_details[[12]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[12]]$move_learn_method
    ## $moves[[51]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[12]]$version_group
    ## $moves[[51]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[51]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[13]]
    ## $moves[[51]]$version_group_details[[13]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[13]]$move_learn_method
    ## $moves[[51]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[13]]$version_group
    ## $moves[[51]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[51]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[14]]
    ## $moves[[51]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[51]]$version_group_details[[14]]$move_learn_method
    ## $moves[[51]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[51]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[14]]$version_group
    ## $moves[[51]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[51]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[15]]
    ## $moves[[51]]$version_group_details[[15]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[15]]$move_learn_method
    ## $moves[[51]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[15]]$version_group
    ## $moves[[51]]$version_group_details[[15]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[51]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[16]]
    ## $moves[[51]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[51]]$version_group_details[[16]]$move_learn_method
    ## $moves[[51]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[51]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[16]]$version_group
    ## $moves[[51]]$version_group_details[[16]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[51]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[17]]
    ## $moves[[51]]$version_group_details[[17]]$level_learned_at
    ## [1] 25
    ## 
    ## $moves[[51]]$version_group_details[[17]]$move_learn_method
    ## $moves[[51]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[17]]$version_group
    ## $moves[[51]]$version_group_details[[17]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[51]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[18]]
    ## $moves[[51]]$version_group_details[[18]]$level_learned_at
    ## [1] 21
    ## 
    ## $moves[[51]]$version_group_details[[18]]$move_learn_method
    ## $moves[[51]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[51]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[51]]$version_group_details[[18]]$version_group
    ## $moves[[51]]$version_group_details[[18]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[51]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[52]]
    ## $moves[[52]]$move
    ## $moves[[52]]$move$name
    ## [1] "synthesis"
    ## 
    ## $moves[[52]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/235/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details
    ## $moves[[52]]$version_group_details[[1]]
    ## $moves[[52]]$version_group_details[[1]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[1]]$move_learn_method
    ## $moves[[52]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[1]]$version_group
    ## $moves[[52]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[52]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[2]]
    ## $moves[[52]]$version_group_details[[2]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[2]]$move_learn_method
    ## $moves[[52]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[2]]$version_group
    ## $moves[[52]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[52]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[3]]
    ## $moves[[52]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[52]]$version_group_details[[3]]$move_learn_method
    ## $moves[[52]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[52]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[3]]$version_group
    ## $moves[[52]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[52]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[4]]
    ## $moves[[52]]$version_group_details[[4]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[4]]$move_learn_method
    ## $moves[[52]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[4]]$version_group
    ## $moves[[52]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[52]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[5]]
    ## $moves[[52]]$version_group_details[[5]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[5]]$move_learn_method
    ## $moves[[52]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[5]]$version_group
    ## $moves[[52]]$version_group_details[[5]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[52]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[6]]
    ## $moves[[52]]$version_group_details[[6]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[6]]$move_learn_method
    ## $moves[[52]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[6]]$version_group
    ## $moves[[52]]$version_group_details[[6]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[52]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[7]]
    ## $moves[[52]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[52]]$version_group_details[[7]]$move_learn_method
    ## $moves[[52]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[52]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[7]]$version_group
    ## $moves[[52]]$version_group_details[[7]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[52]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[8]]
    ## $moves[[52]]$version_group_details[[8]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[8]]$move_learn_method
    ## $moves[[52]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[8]]$version_group
    ## $moves[[52]]$version_group_details[[8]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[52]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[9]]
    ## $moves[[52]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[52]]$version_group_details[[9]]$move_learn_method
    ## $moves[[52]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[52]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[9]]$version_group
    ## $moves[[52]]$version_group_details[[9]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[52]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[10]]
    ## $moves[[52]]$version_group_details[[10]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[10]]$move_learn_method
    ## $moves[[52]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[10]]$version_group
    ## $moves[[52]]$version_group_details[[10]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[52]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[11]]
    ## $moves[[52]]$version_group_details[[11]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[11]]$move_learn_method
    ## $moves[[52]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[11]]$version_group
    ## $moves[[52]]$version_group_details[[11]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[52]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[12]]
    ## $moves[[52]]$version_group_details[[12]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[12]]$move_learn_method
    ## $moves[[52]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[12]]$version_group
    ## $moves[[52]]$version_group_details[[12]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[52]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[13]]
    ## $moves[[52]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[52]]$version_group_details[[13]]$move_learn_method
    ## $moves[[52]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[52]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[13]]$version_group
    ## $moves[[52]]$version_group_details[[13]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[52]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[14]]
    ## $moves[[52]]$version_group_details[[14]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[14]]$move_learn_method
    ## $moves[[52]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[14]]$version_group
    ## $moves[[52]]$version_group_details[[14]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[52]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[15]]
    ## $moves[[52]]$version_group_details[[15]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[15]]$move_learn_method
    ## $moves[[52]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[15]]$version_group
    ## $moves[[52]]$version_group_details[[15]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[52]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[16]]
    ## $moves[[52]]$version_group_details[[16]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[16]]$move_learn_method
    ## $moves[[52]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[16]]$version_group
    ## $moves[[52]]$version_group_details[[16]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[52]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[17]]
    ## $moves[[52]]$version_group_details[[17]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[17]]$move_learn_method
    ## $moves[[52]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[17]]$version_group
    ## $moves[[52]]$version_group_details[[17]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[52]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[18]]
    ## $moves[[52]]$version_group_details[[18]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[18]]$move_learn_method
    ## $moves[[52]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[18]]$version_group
    ## $moves[[52]]$version_group_details[[18]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[52]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[19]]
    ## $moves[[52]]$version_group_details[[19]]$level_learned_at
    ## [1] 39
    ## 
    ## $moves[[52]]$version_group_details[[19]]$move_learn_method
    ## $moves[[52]]$version_group_details[[19]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[19]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[19]]$version_group
    ## $moves[[52]]$version_group_details[[19]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[52]]$version_group_details[[19]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[20]]
    ## $moves[[52]]$version_group_details[[20]]$level_learned_at
    ## [1] 33
    ## 
    ## $moves[[52]]$version_group_details[[20]]$move_learn_method
    ## $moves[[52]]$version_group_details[[20]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[52]]$version_group_details[[20]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[52]]$version_group_details[[20]]$version_group
    ## $moves[[52]]$version_group_details[[20]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[52]]$version_group_details[[20]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[53]]
    ## $moves[[53]]$move
    ## $moves[[53]]$move$name
    ## [1] "hidden-power"
    ## 
    ## $moves[[53]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/237/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details
    ## $moves[[53]]$version_group_details[[1]]
    ## $moves[[53]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[1]]$move_learn_method
    ## $moves[[53]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[1]]$version_group
    ## $moves[[53]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[53]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[2]]
    ## $moves[[53]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[2]]$move_learn_method
    ## $moves[[53]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[2]]$version_group
    ## $moves[[53]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[53]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[3]]
    ## $moves[[53]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[3]]$move_learn_method
    ## $moves[[53]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[3]]$version_group
    ## $moves[[53]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[53]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[4]]
    ## $moves[[53]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[4]]$move_learn_method
    ## $moves[[53]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[4]]$version_group
    ## $moves[[53]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[53]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[5]]
    ## $moves[[53]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[5]]$move_learn_method
    ## $moves[[53]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[5]]$version_group
    ## $moves[[53]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[53]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[6]]
    ## $moves[[53]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[6]]$move_learn_method
    ## $moves[[53]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[6]]$version_group
    ## $moves[[53]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[53]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[7]]
    ## $moves[[53]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[7]]$move_learn_method
    ## $moves[[53]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[7]]$version_group
    ## $moves[[53]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[53]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[8]]
    ## $moves[[53]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[8]]$move_learn_method
    ## $moves[[53]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[8]]$version_group
    ## $moves[[53]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[53]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[9]]
    ## $moves[[53]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[9]]$move_learn_method
    ## $moves[[53]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[9]]$version_group
    ## $moves[[53]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[53]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[10]]
    ## $moves[[53]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[10]]$move_learn_method
    ## $moves[[53]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[10]]$version_group
    ## $moves[[53]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[53]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[11]]
    ## $moves[[53]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[11]]$move_learn_method
    ## $moves[[53]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[11]]$version_group
    ## $moves[[53]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[53]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[12]]
    ## $moves[[53]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[12]]$move_learn_method
    ## $moves[[53]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[12]]$version_group
    ## $moves[[53]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[53]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[13]]
    ## $moves[[53]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[13]]$move_learn_method
    ## $moves[[53]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[13]]$version_group
    ## $moves[[53]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[53]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[14]]
    ## $moves[[53]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[14]]$move_learn_method
    ## $moves[[53]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[14]]$version_group
    ## $moves[[53]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[53]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[15]]
    ## $moves[[53]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[15]]$move_learn_method
    ## $moves[[53]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[15]]$version_group
    ## $moves[[53]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[53]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[16]]
    ## $moves[[53]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[53]]$version_group_details[[16]]$move_learn_method
    ## $moves[[53]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[53]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[53]]$version_group_details[[16]]$version_group
    ## $moves[[53]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[53]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[54]]
    ## $moves[[54]]$move
    ## $moves[[54]]$move$name
    ## [1] "sunny-day"
    ## 
    ## $moves[[54]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/241/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details
    ## $moves[[54]]$version_group_details[[1]]
    ## $moves[[54]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[1]]$move_learn_method
    ## $moves[[54]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[1]]$version_group
    ## $moves[[54]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[54]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[2]]
    ## $moves[[54]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[2]]$move_learn_method
    ## $moves[[54]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[2]]$version_group
    ## $moves[[54]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[54]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[3]]
    ## $moves[[54]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[3]]$move_learn_method
    ## $moves[[54]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[3]]$version_group
    ## $moves[[54]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[54]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[4]]
    ## $moves[[54]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[4]]$move_learn_method
    ## $moves[[54]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[4]]$version_group
    ## $moves[[54]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[54]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[5]]
    ## $moves[[54]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[5]]$move_learn_method
    ## $moves[[54]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[5]]$version_group
    ## $moves[[54]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[54]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[6]]
    ## $moves[[54]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[6]]$move_learn_method
    ## $moves[[54]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[6]]$version_group
    ## $moves[[54]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[54]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[7]]
    ## $moves[[54]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[7]]$move_learn_method
    ## $moves[[54]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[7]]$version_group
    ## $moves[[54]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[54]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[8]]
    ## $moves[[54]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[8]]$move_learn_method
    ## $moves[[54]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[8]]$version_group
    ## $moves[[54]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[54]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[9]]
    ## $moves[[54]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[9]]$move_learn_method
    ## $moves[[54]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[9]]$version_group
    ## $moves[[54]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[54]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[10]]
    ## $moves[[54]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[10]]$move_learn_method
    ## $moves[[54]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[10]]$version_group
    ## $moves[[54]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[54]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[11]]
    ## $moves[[54]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[11]]$move_learn_method
    ## $moves[[54]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[11]]$version_group
    ## $moves[[54]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[54]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[12]]
    ## $moves[[54]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[12]]$move_learn_method
    ## $moves[[54]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[12]]$version_group
    ## $moves[[54]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[54]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[13]]
    ## $moves[[54]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[13]]$move_learn_method
    ## $moves[[54]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[13]]$version_group
    ## $moves[[54]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[54]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[14]]
    ## $moves[[54]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[14]]$move_learn_method
    ## $moves[[54]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[14]]$version_group
    ## $moves[[54]]$version_group_details[[14]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[54]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[15]]
    ## $moves[[54]]$version_group_details[[15]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[15]]$move_learn_method
    ## $moves[[54]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[15]]$version_group
    ## $moves[[54]]$version_group_details[[15]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[54]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[16]]
    ## $moves[[54]]$version_group_details[[16]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[54]]$version_group_details[[16]]$move_learn_method
    ## $moves[[54]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[54]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[54]]$version_group_details[[16]]$version_group
    ## $moves[[54]]$version_group_details[[16]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[54]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[55]]
    ## $moves[[55]]$move
    ## $moves[[55]]$move$name
    ## [1] "rock-smash"
    ## 
    ## $moves[[55]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/249/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details
    ## $moves[[55]]$version_group_details[[1]]
    ## $moves[[55]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[1]]$move_learn_method
    ## $moves[[55]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[1]]$version_group
    ## $moves[[55]]$version_group_details[[1]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[55]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[2]]
    ## $moves[[55]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[2]]$move_learn_method
    ## $moves[[55]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[2]]$version_group
    ## $moves[[55]]$version_group_details[[2]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[55]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[3]]
    ## $moves[[55]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[3]]$move_learn_method
    ## $moves[[55]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[3]]$version_group
    ## $moves[[55]]$version_group_details[[3]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[55]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[4]]
    ## $moves[[55]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[4]]$move_learn_method
    ## $moves[[55]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[4]]$version_group
    ## $moves[[55]]$version_group_details[[4]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[55]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[5]]
    ## $moves[[55]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[5]]$move_learn_method
    ## $moves[[55]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[5]]$version_group
    ## $moves[[55]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[55]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[6]]
    ## $moves[[55]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[6]]$move_learn_method
    ## $moves[[55]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[6]]$version_group
    ## $moves[[55]]$version_group_details[[6]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[55]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[7]]
    ## $moves[[55]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[7]]$move_learn_method
    ## $moves[[55]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[7]]$version_group
    ## $moves[[55]]$version_group_details[[7]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[55]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[8]]
    ## $moves[[55]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[8]]$move_learn_method
    ## $moves[[55]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[8]]$version_group
    ## $moves[[55]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[55]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[9]]
    ## $moves[[55]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[9]]$move_learn_method
    ## $moves[[55]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[9]]$version_group
    ## $moves[[55]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[55]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[10]]
    ## $moves[[55]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[10]]$move_learn_method
    ## $moves[[55]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[10]]$version_group
    ## $moves[[55]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[55]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[11]]
    ## $moves[[55]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[11]]$move_learn_method
    ## $moves[[55]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[11]]$version_group
    ## $moves[[55]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[55]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[12]]
    ## $moves[[55]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[55]]$version_group_details[[12]]$move_learn_method
    ## $moves[[55]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[55]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[55]]$version_group_details[[12]]$version_group
    ## $moves[[55]]$version_group_details[[12]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[55]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[56]]
    ## $moves[[56]]$move
    ## $moves[[56]]$move$name
    ## [1] "facade"
    ## 
    ## $moves[[56]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/263/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details
    ## $moves[[56]]$version_group_details[[1]]
    ## $moves[[56]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[1]]$move_learn_method
    ## $moves[[56]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[1]]$version_group
    ## $moves[[56]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[56]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[2]]
    ## $moves[[56]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[2]]$move_learn_method
    ## $moves[[56]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[2]]$version_group
    ## $moves[[56]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[56]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[3]]
    ## $moves[[56]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[3]]$move_learn_method
    ## $moves[[56]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[3]]$version_group
    ## $moves[[56]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[56]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[4]]
    ## $moves[[56]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[4]]$move_learn_method
    ## $moves[[56]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[4]]$version_group
    ## $moves[[56]]$version_group_details[[4]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[56]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[5]]
    ## $moves[[56]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[5]]$move_learn_method
    ## $moves[[56]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[5]]$version_group
    ## $moves[[56]]$version_group_details[[5]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[56]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[6]]
    ## $moves[[56]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[6]]$move_learn_method
    ## $moves[[56]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[6]]$version_group
    ## $moves[[56]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[56]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[7]]
    ## $moves[[56]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[7]]$move_learn_method
    ## $moves[[56]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[7]]$version_group
    ## $moves[[56]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[56]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[8]]
    ## $moves[[56]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[8]]$move_learn_method
    ## $moves[[56]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[8]]$version_group
    ## $moves[[56]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[56]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[9]]
    ## $moves[[56]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[9]]$move_learn_method
    ## $moves[[56]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[9]]$version_group
    ## $moves[[56]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[56]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[10]]
    ## $moves[[56]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[10]]$move_learn_method
    ## $moves[[56]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[10]]$version_group
    ## $moves[[56]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[56]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[11]]
    ## $moves[[56]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[11]]$move_learn_method
    ## $moves[[56]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[11]]$version_group
    ## $moves[[56]]$version_group_details[[11]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[56]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[12]]
    ## $moves[[56]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[12]]$move_learn_method
    ## $moves[[56]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[12]]$version_group
    ## $moves[[56]]$version_group_details[[12]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[56]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[13]]
    ## $moves[[56]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[13]]$move_learn_method
    ## $moves[[56]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[13]]$version_group
    ## $moves[[56]]$version_group_details[[13]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[56]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[14]]
    ## $moves[[56]]$version_group_details[[14]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[56]]$version_group_details[[14]]$move_learn_method
    ## $moves[[56]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[56]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[56]]$version_group_details[[14]]$version_group
    ## $moves[[56]]$version_group_details[[14]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[56]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[57]]
    ## $moves[[57]]$move
    ## $moves[[57]]$move$name
    ## [1] "nature-power"
    ## 
    ## $moves[[57]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/267/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details
    ## $moves[[57]]$version_group_details[[1]]
    ## $moves[[57]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[1]]$move_learn_method
    ## $moves[[57]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[57]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[1]]$version_group
    ## $moves[[57]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[57]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[2]]
    ## $moves[[57]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[2]]$move_learn_method
    ## $moves[[57]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[2]]$version_group
    ## $moves[[57]]$version_group_details[[2]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[57]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[3]]
    ## $moves[[57]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[3]]$move_learn_method
    ## $moves[[57]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[57]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[3]]$version_group
    ## $moves[[57]]$version_group_details[[3]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[57]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[4]]
    ## $moves[[57]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[4]]$move_learn_method
    ## $moves[[57]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[4]]$version_group
    ## $moves[[57]]$version_group_details[[4]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[57]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[5]]
    ## $moves[[57]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[5]]$move_learn_method
    ## $moves[[57]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[5]]$version_group
    ## $moves[[57]]$version_group_details[[5]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[57]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[6]]
    ## $moves[[57]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[6]]$move_learn_method
    ## $moves[[57]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[6]]$version_group
    ## $moves[[57]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[57]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[7]]
    ## $moves[[57]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[7]]$move_learn_method
    ## $moves[[57]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[7]]$version_group
    ## $moves[[57]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[57]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[8]]
    ## $moves[[57]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[8]]$move_learn_method
    ## $moves[[57]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[8]]$version_group
    ## $moves[[57]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[57]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[9]]
    ## $moves[[57]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[9]]$move_learn_method
    ## $moves[[57]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[57]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[9]]$version_group
    ## $moves[[57]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[57]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[10]]
    ## $moves[[57]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[10]]$move_learn_method
    ## $moves[[57]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[10]]$version_group
    ## $moves[[57]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[57]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[11]]
    ## $moves[[57]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[11]]$move_learn_method
    ## $moves[[57]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[57]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[11]]$version_group
    ## $moves[[57]]$version_group_details[[11]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[57]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[12]]
    ## $moves[[57]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[12]]$move_learn_method
    ## $moves[[57]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[12]]$version_group
    ## $moves[[57]]$version_group_details[[12]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[57]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[13]]
    ## $moves[[57]]$version_group_details[[13]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[57]]$version_group_details[[13]]$move_learn_method
    ## $moves[[57]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[57]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[57]]$version_group_details[[13]]$version_group
    ## $moves[[57]]$version_group_details[[13]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[57]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[58]]
    ## $moves[[58]]$move
    ## $moves[[58]]$move$name
    ## [1] "ingrain"
    ## 
    ## $moves[[58]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/275/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details
    ## $moves[[58]]$version_group_details[[1]]
    ## $moves[[58]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[1]]$move_learn_method
    ## $moves[[58]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[1]]$version_group
    ## $moves[[58]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[58]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[2]]
    ## $moves[[58]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[2]]$move_learn_method
    ## $moves[[58]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[2]]$version_group
    ## $moves[[58]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[58]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[3]]
    ## $moves[[58]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[3]]$move_learn_method
    ## $moves[[58]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[3]]$version_group
    ## $moves[[58]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[58]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[4]]
    ## $moves[[58]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[4]]$move_learn_method
    ## $moves[[58]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[4]]$version_group
    ## $moves[[58]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[58]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[5]]
    ## $moves[[58]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[5]]$move_learn_method
    ## $moves[[58]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[5]]$version_group
    ## $moves[[58]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[58]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[6]]
    ## $moves[[58]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[6]]$move_learn_method
    ## $moves[[58]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[6]]$version_group
    ## $moves[[58]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[58]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[7]]
    ## $moves[[58]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[7]]$move_learn_method
    ## $moves[[58]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[7]]$version_group
    ## $moves[[58]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[58]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[8]]
    ## $moves[[58]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[8]]$move_learn_method
    ## $moves[[58]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[8]]$version_group
    ## $moves[[58]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[58]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[9]]
    ## $moves[[58]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[58]]$version_group_details[[9]]$move_learn_method
    ## $moves[[58]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[58]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[58]]$version_group_details[[9]]$version_group
    ## $moves[[58]]$version_group_details[[9]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[58]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[59]]
    ## $moves[[59]]$move
    ## $moves[[59]]$move$name
    ## [1] "knock-off"
    ## 
    ## $moves[[59]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/282/"
    ## 
    ## 
    ## $moves[[59]]$version_group_details
    ## $moves[[59]]$version_group_details[[1]]
    ## $moves[[59]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[59]]$version_group_details[[1]]$move_learn_method
    ## $moves[[59]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[59]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[1]]$version_group
    ## $moves[[59]]$version_group_details[[1]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[59]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[2]]
    ## $moves[[59]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[59]]$version_group_details[[2]]$move_learn_method
    ## $moves[[59]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[59]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[2]]$version_group
    ## $moves[[59]]$version_group_details[[2]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[59]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[3]]
    ## $moves[[59]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[59]]$version_group_details[[3]]$move_learn_method
    ## $moves[[59]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[59]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[3]]$version_group
    ## $moves[[59]]$version_group_details[[3]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[59]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[4]]
    ## $moves[[59]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[59]]$version_group_details[[4]]$move_learn_method
    ## $moves[[59]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[59]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[59]]$version_group_details[[4]]$version_group
    ## $moves[[59]]$version_group_details[[4]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[59]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[60]]
    ## $moves[[60]]$move
    ## $moves[[60]]$move$name
    ## [1] "secret-power"
    ## 
    ## $moves[[60]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/290/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details
    ## $moves[[60]]$version_group_details[[1]]
    ## $moves[[60]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[1]]$move_learn_method
    ## $moves[[60]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[1]]$version_group
    ## $moves[[60]]$version_group_details[[1]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[60]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[2]]
    ## $moves[[60]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[2]]$move_learn_method
    ## $moves[[60]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[2]]$version_group
    ## $moves[[60]]$version_group_details[[2]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[60]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[3]]
    ## $moves[[60]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[3]]$move_learn_method
    ## $moves[[60]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[3]]$version_group
    ## $moves[[60]]$version_group_details[[3]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[60]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[4]]
    ## $moves[[60]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[4]]$move_learn_method
    ## $moves[[60]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[4]]$version_group
    ## $moves[[60]]$version_group_details[[4]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[60]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[5]]
    ## $moves[[60]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[5]]$move_learn_method
    ## $moves[[60]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[5]]$version_group
    ## $moves[[60]]$version_group_details[[5]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[60]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[6]]
    ## $moves[[60]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[6]]$move_learn_method
    ## $moves[[60]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[6]]$version_group
    ## $moves[[60]]$version_group_details[[6]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[60]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[7]]
    ## $moves[[60]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[7]]$move_learn_method
    ## $moves[[60]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[7]]$version_group
    ## $moves[[60]]$version_group_details[[7]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[60]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[8]]
    ## $moves[[60]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[8]]$move_learn_method
    ## $moves[[60]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[8]]$version_group
    ## $moves[[60]]$version_group_details[[8]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[60]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[9]]
    ## $moves[[60]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[60]]$version_group_details[[9]]$move_learn_method
    ## $moves[[60]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[60]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[60]]$version_group_details[[9]]$version_group
    ## $moves[[60]]$version_group_details[[9]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[60]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[61]]
    ## $moves[[61]]$move
    ## $moves[[61]]$move$name
    ## [1] "grass-whistle"
    ## 
    ## $moves[[61]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/320/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details
    ## $moves[[61]]$version_group_details[[1]]
    ## $moves[[61]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[1]]$move_learn_method
    ## $moves[[61]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[1]]$version_group
    ## $moves[[61]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[61]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[2]]
    ## $moves[[61]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[2]]$move_learn_method
    ## $moves[[61]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[2]]$version_group
    ## $moves[[61]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[61]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[3]]
    ## $moves[[61]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[3]]$move_learn_method
    ## $moves[[61]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[3]]$version_group
    ## $moves[[61]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[61]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[4]]
    ## $moves[[61]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[4]]$move_learn_method
    ## $moves[[61]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[4]]$version_group
    ## $moves[[61]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[61]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[5]]
    ## $moves[[61]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[5]]$move_learn_method
    ## $moves[[61]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[5]]$version_group
    ## $moves[[61]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[61]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[6]]
    ## $moves[[61]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[6]]$move_learn_method
    ## $moves[[61]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[6]]$version_group
    ## $moves[[61]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[61]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[7]]
    ## $moves[[61]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[7]]$move_learn_method
    ## $moves[[61]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[7]]$version_group
    ## $moves[[61]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[61]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[8]]
    ## $moves[[61]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[8]]$move_learn_method
    ## $moves[[61]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[8]]$version_group
    ## $moves[[61]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[61]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[9]]
    ## $moves[[61]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[9]]$move_learn_method
    ## $moves[[61]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[9]]$version_group
    ## $moves[[61]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[61]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[10]]
    ## $moves[[61]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[10]]$move_learn_method
    ## $moves[[61]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[10]]$version_group
    ## $moves[[61]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[61]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[11]]
    ## $moves[[61]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[11]]$move_learn_method
    ## $moves[[61]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[11]]$version_group
    ## $moves[[61]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[61]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[12]]
    ## $moves[[61]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[61]]$version_group_details[[12]]$move_learn_method
    ## $moves[[61]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[61]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[61]]$version_group_details[[12]]$version_group
    ## $moves[[61]]$version_group_details[[12]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[61]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[62]]
    ## $moves[[62]]$move
    ## $moves[[62]]$move$name
    ## [1] "bullet-seed"
    ## 
    ## $moves[[62]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/331/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details
    ## $moves[[62]]$version_group_details[[1]]
    ## $moves[[62]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[1]]$move_learn_method
    ## $moves[[62]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[1]]$version_group
    ## $moves[[62]]$version_group_details[[1]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[62]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[2]]
    ## $moves[[62]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[2]]$move_learn_method
    ## $moves[[62]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[2]]$version_group
    ## $moves[[62]]$version_group_details[[2]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[62]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[3]]
    ## $moves[[62]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[3]]$move_learn_method
    ## $moves[[62]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[3]]$version_group
    ## $moves[[62]]$version_group_details[[3]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[62]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[4]]
    ## $moves[[62]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[4]]$move_learn_method
    ## $moves[[62]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[4]]$version_group
    ## $moves[[62]]$version_group_details[[4]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[62]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[5]]
    ## $moves[[62]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[5]]$move_learn_method
    ## $moves[[62]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[5]]$version_group
    ## $moves[[62]]$version_group_details[[5]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[62]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[6]]
    ## $moves[[62]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[6]]$move_learn_method
    ## $moves[[62]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[6]]$version_group
    ## $moves[[62]]$version_group_details[[6]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[62]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[7]]
    ## $moves[[62]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[7]]$move_learn_method
    ## $moves[[62]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[7]]$version_group
    ## $moves[[62]]$version_group_details[[7]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[62]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[8]]
    ## $moves[[62]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[62]]$version_group_details[[8]]$move_learn_method
    ## $moves[[62]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[62]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[62]]$version_group_details[[8]]$version_group
    ## $moves[[62]]$version_group_details[[8]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[62]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[63]]
    ## $moves[[63]]$move
    ## $moves[[63]]$move$name
    ## [1] "magical-leaf"
    ## 
    ## $moves[[63]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/345/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details
    ## $moves[[63]]$version_group_details[[1]]
    ## $moves[[63]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[1]]$move_learn_method
    ## $moves[[63]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[1]]$version_group
    ## $moves[[63]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[63]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[2]]
    ## $moves[[63]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[2]]$move_learn_method
    ## $moves[[63]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[2]]$version_group
    ## $moves[[63]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[63]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[3]]
    ## $moves[[63]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[3]]$move_learn_method
    ## $moves[[63]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[3]]$version_group
    ## $moves[[63]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[63]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[4]]
    ## $moves[[63]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[4]]$move_learn_method
    ## $moves[[63]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[4]]$version_group
    ## $moves[[63]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[63]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[5]]
    ## $moves[[63]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[5]]$move_learn_method
    ## $moves[[63]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[5]]$version_group
    ## $moves[[63]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[63]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[6]]
    ## $moves[[63]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[6]]$move_learn_method
    ## $moves[[63]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[6]]$version_group
    ## $moves[[63]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[63]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[7]]
    ## $moves[[63]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[7]]$move_learn_method
    ## $moves[[63]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[7]]$version_group
    ## $moves[[63]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[63]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[8]]
    ## $moves[[63]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[8]]$move_learn_method
    ## $moves[[63]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[8]]$version_group
    ## $moves[[63]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[63]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[9]]
    ## $moves[[63]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[9]]$move_learn_method
    ## $moves[[63]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[9]]$version_group
    ## $moves[[63]]$version_group_details[[9]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[63]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[10]]
    ## $moves[[63]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[10]]$move_learn_method
    ## $moves[[63]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[10]]$version_group
    ## $moves[[63]]$version_group_details[[10]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[63]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[11]]
    ## $moves[[63]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[11]]$move_learn_method
    ## $moves[[63]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[11]]$version_group
    ## $moves[[63]]$version_group_details[[11]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[63]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[12]]
    ## $moves[[63]]$version_group_details[[12]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[63]]$version_group_details[[12]]$move_learn_method
    ## $moves[[63]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[63]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[63]]$version_group_details[[12]]$version_group
    ## $moves[[63]]$version_group_details[[12]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[63]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[64]]
    ## $moves[[64]]$move
    ## $moves[[64]]$move$name
    ## [1] "natural-gift"
    ## 
    ## $moves[[64]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/363/"
    ## 
    ## 
    ## $moves[[64]]$version_group_details
    ## $moves[[64]]$version_group_details[[1]]
    ## $moves[[64]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[64]]$version_group_details[[1]]$move_learn_method
    ## $moves[[64]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[64]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[64]]$version_group_details[[1]]$version_group
    ## $moves[[64]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[64]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[64]]$version_group_details[[2]]
    ## $moves[[64]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[64]]$version_group_details[[2]]$move_learn_method
    ## $moves[[64]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[64]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[64]]$version_group_details[[2]]$version_group
    ## $moves[[64]]$version_group_details[[2]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[64]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[64]]$version_group_details[[3]]
    ## $moves[[64]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[64]]$version_group_details[[3]]$move_learn_method
    ## $moves[[64]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[64]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[64]]$version_group_details[[3]]$version_group
    ## $moves[[64]]$version_group_details[[3]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[64]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[65]]
    ## $moves[[65]]$move
    ## $moves[[65]]$move$name
    ## [1] "worry-seed"
    ## 
    ## $moves[[65]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/388/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details
    ## $moves[[65]]$version_group_details[[1]]
    ## $moves[[65]]$version_group_details[[1]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[1]]$move_learn_method
    ## $moves[[65]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[1]]$version_group
    ## $moves[[65]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[65]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[2]]
    ## $moves[[65]]$version_group_details[[2]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[2]]$move_learn_method
    ## $moves[[65]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[2]]$version_group
    ## $moves[[65]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[65]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[3]]
    ## $moves[[65]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[65]]$version_group_details[[3]]$move_learn_method
    ## $moves[[65]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[65]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[3]]$version_group
    ## $moves[[65]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[65]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[4]]
    ## $moves[[65]]$version_group_details[[4]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[4]]$move_learn_method
    ## $moves[[65]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[4]]$version_group
    ## $moves[[65]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[65]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[5]]
    ## $moves[[65]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[65]]$version_group_details[[5]]$move_learn_method
    ## $moves[[65]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[65]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[5]]$version_group
    ## $moves[[65]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[65]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[6]]
    ## $moves[[65]]$version_group_details[[6]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[6]]$move_learn_method
    ## $moves[[65]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[6]]$version_group
    ## $moves[[65]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[65]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[7]]
    ## $moves[[65]]$version_group_details[[7]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[7]]$move_learn_method
    ## $moves[[65]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[7]]$version_group
    ## $moves[[65]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[65]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[8]]
    ## $moves[[65]]$version_group_details[[8]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[8]]$move_learn_method
    ## $moves[[65]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[8]]$version_group
    ## $moves[[65]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[65]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[9]]
    ## $moves[[65]]$version_group_details[[9]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[9]]$move_learn_method
    ## $moves[[65]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[9]]$version_group
    ## $moves[[65]]$version_group_details[[9]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[65]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[10]]
    ## $moves[[65]]$version_group_details[[10]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[65]]$version_group_details[[10]]$move_learn_method
    ## $moves[[65]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[65]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[10]]$version_group
    ## $moves[[65]]$version_group_details[[10]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[65]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[11]]
    ## $moves[[65]]$version_group_details[[11]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[11]]$move_learn_method
    ## $moves[[65]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[11]]$version_group
    ## $moves[[65]]$version_group_details[[11]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[65]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[12]]
    ## $moves[[65]]$version_group_details[[12]]$level_learned_at
    ## [1] 31
    ## 
    ## $moves[[65]]$version_group_details[[12]]$move_learn_method
    ## $moves[[65]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[65]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[65]]$version_group_details[[12]]$version_group
    ## $moves[[65]]$version_group_details[[12]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[65]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[66]]
    ## $moves[[66]]$move
    ## $moves[[66]]$move$name
    ## [1] "seed-bomb"
    ## 
    ## $moves[[66]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/402/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details
    ## $moves[[66]]$version_group_details[[1]]
    ## $moves[[66]]$version_group_details[[1]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[1]]$move_learn_method
    ## $moves[[66]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[1]]$version_group
    ## $moves[[66]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[66]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[2]]
    ## $moves[[66]]$version_group_details[[2]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[2]]$move_learn_method
    ## $moves[[66]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[2]]$version_group
    ## $moves[[66]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[66]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[3]]
    ## $moves[[66]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[66]]$version_group_details[[3]]$move_learn_method
    ## $moves[[66]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[66]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[3]]$version_group
    ## $moves[[66]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[66]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[4]]
    ## $moves[[66]]$version_group_details[[4]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[4]]$move_learn_method
    ## $moves[[66]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[4]]$version_group
    ## $moves[[66]]$version_group_details[[4]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[66]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[5]]
    ## $moves[[66]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[66]]$version_group_details[[5]]$move_learn_method
    ## $moves[[66]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[66]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[5]]$version_group
    ## $moves[[66]]$version_group_details[[5]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[66]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[6]]
    ## $moves[[66]]$version_group_details[[6]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[6]]$move_learn_method
    ## $moves[[66]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[6]]$version_group
    ## $moves[[66]]$version_group_details[[6]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[66]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[7]]
    ## $moves[[66]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[66]]$version_group_details[[7]]$move_learn_method
    ## $moves[[66]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[66]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[7]]$version_group
    ## $moves[[66]]$version_group_details[[7]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[66]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[8]]
    ## $moves[[66]]$version_group_details[[8]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[8]]$move_learn_method
    ## $moves[[66]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[8]]$version_group
    ## $moves[[66]]$version_group_details[[8]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[66]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[9]]
    ## $moves[[66]]$version_group_details[[9]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[9]]$move_learn_method
    ## $moves[[66]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[9]]$version_group
    ## $moves[[66]]$version_group_details[[9]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[66]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[10]]
    ## $moves[[66]]$version_group_details[[10]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[10]]$move_learn_method
    ## $moves[[66]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[10]]$version_group
    ## $moves[[66]]$version_group_details[[10]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[66]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[11]]
    ## $moves[[66]]$version_group_details[[11]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[66]]$version_group_details[[11]]$move_learn_method
    ## $moves[[66]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[66]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[11]]$version_group
    ## $moves[[66]]$version_group_details[[11]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[66]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[12]]
    ## $moves[[66]]$version_group_details[[12]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[12]]$move_learn_method
    ## $moves[[66]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[12]]$version_group
    ## $moves[[66]]$version_group_details[[12]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[66]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[13]]
    ## $moves[[66]]$version_group_details[[13]]$level_learned_at
    ## [1] 37
    ## 
    ## $moves[[66]]$version_group_details[[13]]$move_learn_method
    ## $moves[[66]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[66]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[66]]$version_group_details[[13]]$version_group
    ## $moves[[66]]$version_group_details[[13]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[66]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[67]]
    ## $moves[[67]]$move
    ## $moves[[67]]$move$name
    ## [1] "energy-ball"
    ## 
    ## $moves[[67]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/412/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details
    ## $moves[[67]]$version_group_details[[1]]
    ## $moves[[67]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[1]]$move_learn_method
    ## $moves[[67]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[1]]$version_group
    ## $moves[[67]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[67]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[2]]
    ## $moves[[67]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[2]]$move_learn_method
    ## $moves[[67]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[2]]$version_group
    ## $moves[[67]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[67]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[3]]
    ## $moves[[67]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[3]]$move_learn_method
    ## $moves[[67]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[3]]$version_group
    ## $moves[[67]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[67]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[4]]
    ## $moves[[67]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[4]]$move_learn_method
    ## $moves[[67]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[4]]$version_group
    ## $moves[[67]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[67]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[5]]
    ## $moves[[67]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[5]]$move_learn_method
    ## $moves[[67]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[5]]$version_group
    ## $moves[[67]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[67]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[6]]
    ## $moves[[67]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[6]]$move_learn_method
    ## $moves[[67]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[6]]$version_group
    ## $moves[[67]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[67]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[7]]
    ## $moves[[67]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[7]]$move_learn_method
    ## $moves[[67]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[7]]$version_group
    ## $moves[[67]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[67]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[8]]
    ## $moves[[67]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[8]]$move_learn_method
    ## $moves[[67]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[8]]$version_group
    ## $moves[[67]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[67]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[9]]
    ## $moves[[67]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[67]]$version_group_details[[9]]$move_learn_method
    ## $moves[[67]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[67]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[67]]$version_group_details[[9]]$version_group
    ## $moves[[67]]$version_group_details[[9]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[67]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[68]]
    ## $moves[[68]]$move
    ## $moves[[68]]$move$name
    ## [1] "leaf-storm"
    ## 
    ## $moves[[68]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/437/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details
    ## $moves[[68]]$version_group_details[[1]]
    ## $moves[[68]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[1]]$move_learn_method
    ## $moves[[68]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[1]]$version_group
    ## $moves[[68]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[68]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[2]]
    ## $moves[[68]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[2]]$move_learn_method
    ## $moves[[68]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[2]]$version_group
    ## $moves[[68]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[68]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[3]]
    ## $moves[[68]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[3]]$move_learn_method
    ## $moves[[68]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[3]]$version_group
    ## $moves[[68]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[68]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[4]]
    ## $moves[[68]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[4]]$move_learn_method
    ## $moves[[68]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[4]]$version_group
    ## $moves[[68]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[68]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[5]]
    ## $moves[[68]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[5]]$move_learn_method
    ## $moves[[68]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[5]]$version_group
    ## $moves[[68]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[68]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[6]]
    ## $moves[[68]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[6]]$move_learn_method
    ## $moves[[68]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[6]]$version_group
    ## $moves[[68]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[68]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[7]]
    ## $moves[[68]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[7]]$move_learn_method
    ## $moves[[68]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[7]]$version_group
    ## $moves[[68]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[68]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[8]]
    ## $moves[[68]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[8]]$move_learn_method
    ## $moves[[68]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[8]]$version_group
    ## $moves[[68]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[68]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[9]]
    ## $moves[[68]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[68]]$version_group_details[[9]]$move_learn_method
    ## $moves[[68]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[68]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[68]]$version_group_details[[9]]$version_group
    ## $moves[[68]]$version_group_details[[9]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[68]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[69]]
    ## $moves[[69]]$move
    ## $moves[[69]]$move$name
    ## [1] "power-whip"
    ## 
    ## $moves[[69]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/438/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details
    ## $moves[[69]]$version_group_details[[1]]
    ## $moves[[69]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[1]]$move_learn_method
    ## $moves[[69]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[1]]$version_group
    ## $moves[[69]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[69]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[2]]
    ## $moves[[69]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[2]]$move_learn_method
    ## $moves[[69]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[2]]$version_group
    ## $moves[[69]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[69]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[3]]
    ## $moves[[69]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[3]]$move_learn_method
    ## $moves[[69]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[3]]$version_group
    ## $moves[[69]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[69]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[4]]
    ## $moves[[69]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[4]]$move_learn_method
    ## $moves[[69]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[4]]$version_group
    ## $moves[[69]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[69]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[5]]
    ## $moves[[69]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[5]]$move_learn_method
    ## $moves[[69]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[5]]$version_group
    ## $moves[[69]]$version_group_details[[5]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[69]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[6]]
    ## $moves[[69]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[6]]$move_learn_method
    ## $moves[[69]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[6]]$version_group
    ## $moves[[69]]$version_group_details[[6]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[69]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[7]]
    ## $moves[[69]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[69]]$version_group_details[[7]]$move_learn_method
    ## $moves[[69]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[69]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[69]]$version_group_details[[7]]$version_group
    ## $moves[[69]]$version_group_details[[7]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[69]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[70]]
    ## $moves[[70]]$move
    ## $moves[[70]]$move$name
    ## [1] "captivate"
    ## 
    ## $moves[[70]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/445/"
    ## 
    ## 
    ## $moves[[70]]$version_group_details
    ## $moves[[70]]$version_group_details[[1]]
    ## $moves[[70]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[70]]$version_group_details[[1]]$move_learn_method
    ## $moves[[70]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[70]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[70]]$version_group_details[[1]]$version_group
    ## $moves[[70]]$version_group_details[[1]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[70]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[70]]$version_group_details[[2]]
    ## $moves[[70]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[70]]$version_group_details[[2]]$move_learn_method
    ## $moves[[70]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[70]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[70]]$version_group_details[[2]]$version_group
    ## $moves[[70]]$version_group_details[[2]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[70]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[70]]$version_group_details[[3]]
    ## $moves[[70]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[70]]$version_group_details[[3]]$move_learn_method
    ## $moves[[70]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[70]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[70]]$version_group_details[[3]]$version_group
    ## $moves[[70]]$version_group_details[[3]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[70]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[71]]
    ## $moves[[71]]$move
    ## $moves[[71]]$move$name
    ## [1] "grass-knot"
    ## 
    ## $moves[[71]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/447/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details
    ## $moves[[71]]$version_group_details[[1]]
    ## $moves[[71]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[1]]$move_learn_method
    ## $moves[[71]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[1]]$version_group
    ## $moves[[71]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[71]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[2]]
    ## $moves[[71]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[2]]$move_learn_method
    ## $moves[[71]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[2]]$version_group
    ## $moves[[71]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[71]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[3]]
    ## $moves[[71]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[3]]$move_learn_method
    ## $moves[[71]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[3]]$version_group
    ## $moves[[71]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[71]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[4]]
    ## $moves[[71]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[4]]$move_learn_method
    ## $moves[[71]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[4]]$version_group
    ## $moves[[71]]$version_group_details[[4]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[71]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[5]]
    ## $moves[[71]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[5]]$move_learn_method
    ## $moves[[71]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[5]]$version_group
    ## $moves[[71]]$version_group_details[[5]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[71]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[6]]
    ## $moves[[71]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[6]]$move_learn_method
    ## $moves[[71]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[6]]$version_group
    ## $moves[[71]]$version_group_details[[6]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[71]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[7]]
    ## $moves[[71]]$version_group_details[[7]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[7]]$move_learn_method
    ## $moves[[71]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[7]]$version_group
    ## $moves[[71]]$version_group_details[[7]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[71]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[8]]
    ## $moves[[71]]$version_group_details[[8]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[8]]$move_learn_method
    ## $moves[[71]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[8]]$version_group
    ## $moves[[71]]$version_group_details[[8]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[71]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[9]]
    ## $moves[[71]]$version_group_details[[9]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[71]]$version_group_details[[9]]$move_learn_method
    ## $moves[[71]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[71]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[71]]$version_group_details[[9]]$version_group
    ## $moves[[71]]$version_group_details[[9]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[71]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[72]]
    ## $moves[[72]]$move
    ## $moves[[72]]$move$name
    ## [1] "venoshock"
    ## 
    ## $moves[[72]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/474/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details
    ## $moves[[72]]$version_group_details[[1]]
    ## $moves[[72]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[1]]$move_learn_method
    ## $moves[[72]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[1]]$version_group
    ## $moves[[72]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[72]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[2]]
    ## $moves[[72]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[2]]$move_learn_method
    ## $moves[[72]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[2]]$version_group
    ## $moves[[72]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[72]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[3]]
    ## $moves[[72]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[3]]$move_learn_method
    ## $moves[[72]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[3]]$version_group
    ## $moves[[72]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[72]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[4]]
    ## $moves[[72]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[4]]$move_learn_method
    ## $moves[[72]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[4]]$version_group
    ## $moves[[72]]$version_group_details[[4]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[72]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[5]]
    ## $moves[[72]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[5]]$move_learn_method
    ## $moves[[72]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[5]]$version_group
    ## $moves[[72]]$version_group_details[[5]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[72]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[6]]
    ## $moves[[72]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[72]]$version_group_details[[6]]$move_learn_method
    ## $moves[[72]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[72]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[72]]$version_group_details[[6]]$version_group
    ## $moves[[72]]$version_group_details[[6]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[72]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[73]]
    ## $moves[[73]]$move
    ## $moves[[73]]$move$name
    ## [1] "round"
    ## 
    ## $moves[[73]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/496/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details
    ## $moves[[73]]$version_group_details[[1]]
    ## $moves[[73]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[1]]$move_learn_method
    ## $moves[[73]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[1]]$version_group
    ## $moves[[73]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[73]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[2]]
    ## $moves[[73]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[2]]$move_learn_method
    ## $moves[[73]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[2]]$version_group
    ## $moves[[73]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[73]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[3]]
    ## $moves[[73]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[3]]$move_learn_method
    ## $moves[[73]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[3]]$version_group
    ## $moves[[73]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[73]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[4]]
    ## $moves[[73]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[4]]$move_learn_method
    ## $moves[[73]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[4]]$version_group
    ## $moves[[73]]$version_group_details[[4]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[73]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[5]]
    ## $moves[[73]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[5]]$move_learn_method
    ## $moves[[73]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[5]]$version_group
    ## $moves[[73]]$version_group_details[[5]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[73]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[6]]
    ## $moves[[73]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[73]]$version_group_details[[6]]$move_learn_method
    ## $moves[[73]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[73]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[73]]$version_group_details[[6]]$version_group
    ## $moves[[73]]$version_group_details[[6]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[73]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[74]]
    ## $moves[[74]]$move
    ## $moves[[74]]$move$name
    ## [1] "echoed-voice"
    ## 
    ## $moves[[74]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/497/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details
    ## $moves[[74]]$version_group_details[[1]]
    ## $moves[[74]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[1]]$move_learn_method
    ## $moves[[74]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[1]]$version_group
    ## $moves[[74]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[74]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[2]]
    ## $moves[[74]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[2]]$move_learn_method
    ## $moves[[74]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[2]]$version_group
    ## $moves[[74]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[74]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[3]]
    ## $moves[[74]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[3]]$move_learn_method
    ## $moves[[74]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[3]]$version_group
    ## $moves[[74]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[74]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[4]]
    ## $moves[[74]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[4]]$move_learn_method
    ## $moves[[74]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[4]]$version_group
    ## $moves[[74]]$version_group_details[[4]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[74]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[5]]
    ## $moves[[74]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[5]]$move_learn_method
    ## $moves[[74]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[5]]$version_group
    ## $moves[[74]]$version_group_details[[5]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[74]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[6]]
    ## $moves[[74]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[74]]$version_group_details[[6]]$move_learn_method
    ## $moves[[74]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[74]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[74]]$version_group_details[[6]]$version_group
    ## $moves[[74]]$version_group_details[[6]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[74]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[75]]
    ## $moves[[75]]$move
    ## $moves[[75]]$move$name
    ## [1] "grass-pledge"
    ## 
    ## $moves[[75]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/520/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details
    ## $moves[[75]]$version_group_details[[1]]
    ## $moves[[75]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[1]]$move_learn_method
    ## $moves[[75]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[1]]$version_group
    ## $moves[[75]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[75]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[2]]
    ## $moves[[75]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[2]]$move_learn_method
    ## $moves[[75]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[2]]$version_group
    ## $moves[[75]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[75]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[3]]
    ## $moves[[75]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[3]]$move_learn_method
    ## $moves[[75]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[3]]$version_group
    ## $moves[[75]]$version_group_details[[3]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[75]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[4]]
    ## $moves[[75]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[4]]$move_learn_method
    ## $moves[[75]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[4]]$version_group
    ## $moves[[75]]$version_group_details[[4]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[75]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[5]]
    ## $moves[[75]]$version_group_details[[5]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[5]]$move_learn_method
    ## $moves[[75]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[5]]$version_group
    ## $moves[[75]]$version_group_details[[5]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[75]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[6]]
    ## $moves[[75]]$version_group_details[[6]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[75]]$version_group_details[[6]]$move_learn_method
    ## $moves[[75]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "tutor"
    ## 
    ## $moves[[75]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/3/"
    ## 
    ## 
    ## $moves[[75]]$version_group_details[[6]]$version_group
    ## $moves[[75]]$version_group_details[[6]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[75]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[76]]
    ## $moves[[76]]$move
    ## $moves[[76]]$move$name
    ## [1] "work-up"
    ## 
    ## $moves[[76]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/526/"
    ## 
    ## 
    ## $moves[[76]]$version_group_details
    ## $moves[[76]]$version_group_details[[1]]
    ## $moves[[76]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[76]]$version_group_details[[1]]$move_learn_method
    ## $moves[[76]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[76]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[76]]$version_group_details[[1]]$version_group
    ## $moves[[76]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[76]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[76]]$version_group_details[[2]]
    ## $moves[[76]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[76]]$version_group_details[[2]]$move_learn_method
    ## $moves[[76]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[76]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[76]]$version_group_details[[2]]$version_group
    ## $moves[[76]]$version_group_details[[2]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[76]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[77]]
    ## $moves[[77]]$move
    ## $moves[[77]]$move$name
    ## [1] "grassy-terrain"
    ## 
    ## $moves[[77]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/580/"
    ## 
    ## 
    ## $moves[[77]]$version_group_details
    ## $moves[[77]]$version_group_details[[1]]
    ## $moves[[77]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[77]]$version_group_details[[1]]$move_learn_method
    ## $moves[[77]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[77]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[1]]$version_group
    ## $moves[[77]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[77]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[2]]
    ## $moves[[77]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[77]]$version_group_details[[2]]$move_learn_method
    ## $moves[[77]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[77]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[2]]$version_group
    ## $moves[[77]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[77]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[3]]
    ## $moves[[77]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[77]]$version_group_details[[3]]$move_learn_method
    ## $moves[[77]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[77]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[3]]$version_group
    ## $moves[[77]]$version_group_details[[3]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[77]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[4]]
    ## $moves[[77]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[77]]$version_group_details[[4]]$move_learn_method
    ## $moves[[77]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "egg"
    ## 
    ## $moves[[77]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/2/"
    ## 
    ## 
    ## $moves[[77]]$version_group_details[[4]]$version_group
    ## $moves[[77]]$version_group_details[[4]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[77]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $moves[[78]]
    ## $moves[[78]]$move
    ## $moves[[78]]$move$name
    ## [1] "confide"
    ## 
    ## $moves[[78]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/590/"
    ## 
    ## 
    ## $moves[[78]]$version_group_details
    ## $moves[[78]]$version_group_details[[1]]
    ## $moves[[78]]$version_group_details[[1]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[78]]$version_group_details[[1]]$move_learn_method
    ## $moves[[78]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[78]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[1]]$version_group
    ## $moves[[78]]$version_group_details[[1]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[78]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[2]]
    ## $moves[[78]]$version_group_details[[2]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[78]]$version_group_details[[2]]$move_learn_method
    ## $moves[[78]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[78]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[2]]$version_group
    ## $moves[[78]]$version_group_details[[2]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[78]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[3]]
    ## $moves[[78]]$version_group_details[[3]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[78]]$version_group_details[[3]]$move_learn_method
    ## $moves[[78]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[78]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[3]]$version_group
    ## $moves[[78]]$version_group_details[[3]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[78]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[4]]
    ## $moves[[78]]$version_group_details[[4]]$level_learned_at
    ## [1] 0
    ## 
    ## $moves[[78]]$version_group_details[[4]]$move_learn_method
    ## $moves[[78]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "machine"
    ## 
    ## $moves[[78]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/4/"
    ## 
    ## 
    ## $moves[[78]]$version_group_details[[4]]$version_group
    ## $moves[[78]]$version_group_details[[4]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[78]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $name
    ## [1] "bulbasaur"
    ## 
    ## $order
    ## [1] 1
    ## 
    ## $species
    ## $species$name
    ## [1] "bulbasaur"
    ## 
    ## $species$url
    ## [1] "https://pokeapi.co/api/v2/pokemon-species/1/"
    ## 
    ## 
    ## $sprites
    ## $sprites$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/1.png"
    ## 
    ## $sprites$back_female
    ## NULL
    ## 
    ## $sprites$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/1.png"
    ## 
    ## $sprites$back_shiny_female
    ## NULL
    ## 
    ## $sprites$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/1.png"
    ## 
    ## $sprites$front_female
    ## NULL
    ## 
    ## $sprites$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/1.png"
    ## 
    ## $sprites$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $stats
    ## $stats[[1]]
    ## $stats[[1]]$base_stat
    ## [1] 45
    ## 
    ## $stats[[1]]$effort
    ## [1] 0
    ## 
    ## $stats[[1]]$stat
    ## $stats[[1]]$stat$name
    ## [1] "speed"
    ## 
    ## $stats[[1]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/6/"
    ## 
    ## 
    ## 
    ## $stats[[2]]
    ## $stats[[2]]$base_stat
    ## [1] 65
    ## 
    ## $stats[[2]]$effort
    ## [1] 0
    ## 
    ## $stats[[2]]$stat
    ## $stats[[2]]$stat$name
    ## [1] "special-defense"
    ## 
    ## $stats[[2]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/5/"
    ## 
    ## 
    ## 
    ## $stats[[3]]
    ## $stats[[3]]$base_stat
    ## [1] 65
    ## 
    ## $stats[[3]]$effort
    ## [1] 1
    ## 
    ## $stats[[3]]$stat
    ## $stats[[3]]$stat$name
    ## [1] "special-attack"
    ## 
    ## $stats[[3]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/4/"
    ## 
    ## 
    ## 
    ## $stats[[4]]
    ## $stats[[4]]$base_stat
    ## [1] 49
    ## 
    ## $stats[[4]]$effort
    ## [1] 0
    ## 
    ## $stats[[4]]$stat
    ## $stats[[4]]$stat$name
    ## [1] "defense"
    ## 
    ## $stats[[4]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/3/"
    ## 
    ## 
    ## 
    ## $stats[[5]]
    ## $stats[[5]]$base_stat
    ## [1] 49
    ## 
    ## $stats[[5]]$effort
    ## [1] 0
    ## 
    ## $stats[[5]]$stat
    ## $stats[[5]]$stat$name
    ## [1] "attack"
    ## 
    ## $stats[[5]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/2/"
    ## 
    ## 
    ## 
    ## $stats[[6]]
    ## $stats[[6]]$base_stat
    ## [1] 45
    ## 
    ## $stats[[6]]$effort
    ## [1] 0
    ## 
    ## $stats[[6]]$stat
    ## $stats[[6]]$stat$name
    ## [1] "hp"
    ## 
    ## $stats[[6]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/1/"
    ## 
    ## 
    ## 
    ## 
    ## $types
    ## $types[[1]]
    ## $types[[1]]$slot
    ## [1] 2
    ## 
    ## $types[[1]]$type
    ## $types[[1]]$type$name
    ## [1] "poison"
    ## 
    ## $types[[1]]$type$url
    ## [1] "https://pokeapi.co/api/v2/type/4/"
    ## 
    ## 
    ## 
    ## $types[[2]]
    ## $types[[2]]$slot
    ## [1] 1
    ## 
    ## $types[[2]]$type
    ## $types[[2]]$type$name
    ## [1] "grass"
    ## 
    ## $types[[2]]$type$url
    ## [1] "https://pokeapi.co/api/v2/type/12/"
    ## 
    ## 
    ## 
    ## 
    ## $weight
    ## [1] 69

``` r
poke$name
```

    ## [1] "bulbasaur"

``` r
poke$height
```

    ## [1] 7

``` r
poke$abilities
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[1]]$slot
    ## [1] 3
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "overgrow"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[2]]$slot
    ## [1] 1

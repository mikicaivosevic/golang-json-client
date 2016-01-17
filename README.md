#Example how to easily consume JSON APIs with Go (golang) 

###Structures
```go
type Geo struct {
	Lat string `json:"lat"`
	Lng string `json:"lng"`
}

type Company struct {
	Name string `json:"name"`
	CatchPhrase string `json:"catchPhrase"`
	Bs string `json:"bs"`
}

type Address struct {
	Street string `json:"street"`
	Suite string `json:"suite"`
	City string `json:"city"`
	ZipCode string `json:"zipcode"`
	Geo Geo `json:"geo"`
}

type User struct {
	Id int `json:"id"`
	Name string `json:"name"`
	Username string `json:"username"`
	Email string `json:"email"`
	Company Company `json:"company"`
	Address Address `json:"address"`
	Phone string `json:"phone"`
	Website string `json:"website"`
}
```

###Function which returns json data from API
```go
func getResponseData(url string) []byte {
	response, err := http.Get(url)
	if err != nil {
		fmt.Print(err)
	}
	defer response.Body.Close()
	contents, _ := ioutil.ReadAll(response.Body)
	return contents
}
```

###Main function
```go
func main() {
	contents := getResponseData("http://jsonplaceholder.typicode.com/users")
	var users []User
	json.Unmarshal(contents, &users)  //Parse JSON data and stores the result in var users

	//users geo location
	for _, user := range users {
		fmt.Printf("%s - Lat: %s Lng: %s \n", user.Name, user.Address.Geo.Lat, user.Address.Geo.Lng)
	}
}
```
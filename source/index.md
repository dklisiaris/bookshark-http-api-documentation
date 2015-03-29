---
title: Bookshark API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Bookshark Home</a>
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Introduction

The __Bookshark API__ is a book _metadata service_ for books published in greek language. Metadata is the is the information (data) which describes other information (in this case: books). Currently all metadata are being extracted from [biblionet.gr](http://biblionet.gr) in real time. 

You can search for a specific book by providing its _isbn_ or its _biblionet id_. You can also perform more _advanced searches_ by providing book's title, author, publishing date etc. and receive a collection of books. Beyond books, information about authors and publishers are available too. 

All __requests__ are done through _http_, so you build your url depending on what you want to search for and do the request. Currently all __responses__ are in _json_ format and you can choose pretty or minified json. 
     
# Audience

This document is intended for website and mobile developers who want to use the bookshark API in order to collect and present information about greek books. 
It provides a guide to using the API and reference material on the available parameters.

# Books

## Get a book

```shell
# These commands extract the same book.
curl "http://bookshark.eu/api/v1/book?isbn=960-14-1157-7"
curl "http://bookshark.eu/api/v1/book?id=103788"
curl "http://bookshark.eu/api/v1/book?uri=http://biblionet.gr/book/103788/"

```

> The above commands return JSON structured like this:

```json
{
  "book": [
    {
      "title": "Σημεία και τέρατα της οικονομίας",
      "subtitle": "Η κρυφή πλευρά των πάντων",
      "image": "http://www.biblionet.gr/images/covers/b103788.jpg",
      "author": [
        {
          "name": "Steven D. Levitt",
          "b_id": "59782"
        },
        {
          "name": "Stephen J. Dubner",
          "b_id": "59783"
        }
      ],
      "contributors": {
        "μετάφραση": [
          {
            "name": "Άγγελος Φιλιππάτος",
            "b_id": "851"
          }
        ]
      },
      "publisher": {
        "name": "Εκδοτικός Οίκος Α. Α. Λιβάνη",
        "b_id": "271"
      },
      "publication_year": "2006",
      "pages": "326",
      "isbn": "960-14-1157-7",
      "isbn_13": "978-960-14-1157-6",
      "status": "Κυκλοφορεί",
      "price": "16,31",
      "award": [
      ],
      "description": "Τι είναι πιο επικίνδυνο, ένα όπλο ή μια πισίνα; Τι κοινό έχουν οι δάσκαλοι με τους παλαιστές του σούμο;...",
      "category": [
        {
          "ddc": "330",
          "name": "Οικονομία",
          "b_id": "142"
        }
      ],
      "b_id": "103788"
    }
  ]
}
```

This endpoint retrieves a specific book, based on its isbn, its biblionet id or url.

### HTTP Request

`GET http://bookshark.eu/api/v1/book?parameters`

### Query Parameters

In each request only one of isbn, id and uri should be used to specify the book.

Parameter | Default | Description
--------- | ------- | -----------
isbn | empty | The book's ISBN code.
id | empty | The book's id in biblionet site.
uri | empty | The book's url in biblionet site.
eager | 0 | If set to 1, it activates eager book extraction.

### Some example requests

Generally the preferable option is getting books by isbn. If the biblionet id is known then thats the fastest method. Here are some request examples which fetch the same book:

`GET http://bookshark.eu/api/v1/book?isbn=960-14-1157-7`

`GET http://bookshark.eu/api/v1/book?id=103788`

`GET http://bookshark.eu/api/v1/book?uri=http://biblionet.gr/book/103788/`

`GET http://bookshark.eu/api/v1/book?isbn=978-960-14-1157-6`

`GET http://bookshark.eu/api/v1/book?isbn=9789601411576`


<aside class="success">
Remember — ISBN can contain dashes or be a plain number. ISBN-13 is also acceptable.
</aside>

### JSON Response

The response is in the format: `{"metadata-type": [{ metadata-hash }]}`
Here are the metadata-hash keys for the book object:

* title
* subtitle
* image
* author (Array of authors or the string 'collective work')
  * name
  * b_id
* contributors (Array of hashes)
  * job-hash (A hash in the form of job_name => [Array of authors])
* publisher
  * name
  * b_id 
* publication_year
* pages
* isbn
* isbn_13 (May be null, some books dont have isbn13)
* issn (May not be there)
* ismn (May not be there, only music books issn)
* status
* price
* award (Array of awards)
  * name
  * year
* description
* category (Array of categories)
  * name
  * ddc (Dewey Decimal Classification)
  * b_id 
* b_id

<aside class="warning">
Be careful — Most of the above values may be null. Always check if a value is present before use.
</aside>

## Eager load a book

```shell
# These commands eager load the same book.
curl "http://bookshark.eu/api/v1/book?isbn=978-960-6640-84-1&eager=1"
curl "http://bookshark.eu/api/v1/book?id=185281&eager=1"
curl "http://bookshark.eu/api/v1/book?uri=http://biblionet.gr/book/185281/&eager=1"

```

> The above commands return JSON structured like this:

```json
{
  "book": [
    {
      "title": "Τα μαθηματικά της ζωής",
      "subtitle": "Ξεκλειδώνοντας τα μυστικά της ύπαρξης",
      "image": "http://www.biblionet.gr/images/covers/b185281.jpg",
      "author": [
        {
          "name": "Stewart, Ian",
          "firstname": "Ian",
          "lastname": "Stewart",
          "extra_info": "1945-",
          "image": "http://www.biblionet.gr/images/persons/4647.jpg",
          "bio": "Ο Ίαν Στιούαρτ γεννήθηκε στην Αγγλία στο Folkestone...",
          "award": [
          ],
          "b_id": "4647"
        }
      ],
      "contributors": {
        "μετάφραση": [
          {
            "name": "Αποστολόπουλος, Νίκος",
            "firstname": "Νίκος",
            "lastname": "Αποστολόπουλος",
            "extra_info": "μεταφραστής",
            "image": null,
            "bio": "",
            "award": [
            ],
            "b_id": "86407"
          }
        ],
        "επιμέλεια": [
          {
            "name": "Τάρτας, Αθανάσιος",
            "firstname": "Αθανάσιος",
            "lastname": "Τάρτας",
            "extra_info": null,
            "image": null,
            "bio": "Ο Αθανάσιος Τάρτας είναι βιολόγος-βιοχημικός, MSc, PhSD.",
            "award": [
            ],
            "b_id": "105693"
          }
        ]
      },
      "publisher": [
        {
          "name": "Τραυλός",
          "owner": "Τραυλός Παναγιώτης",
          "bookstores": {
            "Έδρα": {
              "address": [
                "Καλλιδρομίου 54Α",
                "114 73 Αθήνα"
              ],
              "telephone": [
                "210 3814410",
                "210 3813591"
              ],
              "fax": "210 3828174",
              "email": "travl@acci.gr",
              "website": "www.travlos.gr"
            }
          },
          "b_id": "451"
        }
      ],
      "publication_year": "2012",
      "pages": "572",
      "isbn": "978-960-6640-84-1",
      "isbn_13": null,
      "status": "Κυκλοφορεί",
      "price": "22,00",
      "award": [
      ],
      "description": "...Τα μυστικά της ύπαρξης, η ίδια η φύση της ζωής, δεν είναι απλώς ζήτημα βιοχημείας....",
      "category": [
        {
          "192": {
            "ddc": "500",
            "name": "Φυσικές και θετικές επιστήμες",
            "parent": null
          },
          "8344": {
            "ddc": "500",
            "name": "Φυσικές και θετικές επιστήμες - Γενικά έργα",
            "parent": "192"
          },
          "57": {
            "ddc": "510",
            "name": "Μαθηματικά",
            "parent": "192"
          },
          "_comment": "More child categories are condensed...",
          "1429": {
            "ddc": "590",
            "name": "Ζωολογία",
            "parent": "192"
          },
          "current": {
            "ddc": "500",
            "name": "Φυσικές και θετικές επιστήμες",
            "parent": null,
            "b_id": "192"
          }
        },
        {
          "_comment": "More category trees are condensed..."
        }
      ],
      "b_id": "185281"
    }
  ]
}
```

Each book has some attributes such as authors, contributors, categories etc which are actually references to other objects.
By default when extracting a book, you get only names of these objects and references to their pages.
With eager option set to true, each of these objects' data is extracted and the produced output contains complete information about every object.
You can enable eager loading by set eager parameter to 1.

### Some example requests

`GET http://bookshark.eu/api/v1/book?isbn=960-14-1157-7&eager=1`

`GET http://bookshark.eu/api/v1/book?id=103788&eager=1`

`GET http://bookshark.eu/api/v1/book?isbn=978-960-14-1157-6&eager=1`

### JSON Response

The response is in the same format as the plain book above: 
`{"metadata-type": [{ metadata-hash }]}`, but with more attributes in authors, contributors, publishers and categories.
Here are the metadata-hash keys for the book object:

* title
* subtitle
* image
* author (Array of authors)
  * name
  * firstname
  * lastname
  * extra_info
  * bio
  * image
  * award (Array of awards)
    * name
    * year
  * b_id
* contributors (Array of hashes)
  * job-hash (A hash in the form of job_name => [Array of authors])
* publisher
  * name
  * owner
  * bookstores (A hash of bookstores in the form of bookstore_title => {bookstore-data}})
    * address (Array of addresses)
    * telephone (Array of telephones)
    * fax
    * email
    * website
  * b_id 
* publication_year
* pages
* isbn
* isbn_13 (May be null, some books dont have isbn13)
* issn (May not be there)
* ismn (May not be there, only music books issn)
* status
* price
* award (Array of awards)
  * name
  * year
* description
* category (Array of category hierarchies in form: category-id => category-data)
  * name
  * ddc (Dewey Decimal Classification)
  * b_id 
  * parent
* b_id

<aside class="warning">
Warning — Eager loading of a book is very slow. It is better to normally extract a book and then extract metadata about a specific author or publisher.
</aside>

## Search Books

```shell
# Get books with title ανδρομαχη by author ευριπιδης.
curl "http://bookshark.eu/api/v1/search?title=ανδρομαχη&author=ευριπιδης"
# Get books with title χομπιτ by author τολκιν (setting results to metadata is optional because it is the default type anyway).
curl "http://bookshark.eu/api/v1/search?title=χομπιτ&author=τολκιν&results_type=metadata"
# Get only the ids of books with title χομπιτ by author τολκιν
curl "http://bookshark.eu/api/v1/search?title=αρχοντας&author=τολκιν&results_type=ids"
# Get books by author arthur doyle, published after 2010.
curl "http://bookshark.eu/api/v1/search?author=arthur%20doyle&after_year=2010"
# Get books with isbn 978-960-14-1157-6
curl "http://bookshark.eu/api/v1/search?isbn=978-960-14-1157-6"

```

> Search results create a collection of books, same as if each book has been extracted on its own.

```json
{
  "book": [
    {
      "title": "Στης Χλόης τα απόκρυφα",
      "subtitle": "…και άλλα σημεία και τέρατα",
      "... Rest of Metadata ...": "... condensed ..."
    },
    {
      "title": "Σημεία και τέρατα της οικονομίας",
      "subtitle": "Η κρυφή πλευρά των πάντων",
      "... Rest of Metadata ...": "... condensed ..."     
    },
    {
      "title": "Και άλλα σημεία και τέρατα από την ιστορία",
      "subtitle": null,
      "... Rest of Metadata ...": "... condensed ..."
    },
    {
      "title": "Σημεία και τέρατα από την ιστορία",
      "subtitle": null,
      "... Rest of Metadata ...": "... condensed ..."      
    }
  ]
}
```

> Results with results_type option set to ids look like this:

```json 
{
 "book": [
    "119000",
    "103788",
    "87815",
    "87812",
    "15839",
    "77381",
    "46856",
    "46763",
    "33301"
  ]
}
```

Instead of getting a specific book by providing the book's isbn or id, a search function can be used to get one or more books based on some parameters. This is the only way to search for books when you don't know the isbn or you want to get a collection of more than one books.


### HTTP Request

`GET http://bookshark.eu/api/v1/search?parameters`

### Query Parameters

In each request only one of isbn, id and uri should be used to specify the book.

Parameter | Description
--------- | -----------
isbn | The book's ISBN code.
id | The book's id in biblionet site.
uri | The book's url in biblionet site.
eager | If set to 1, it activates eager book extraction.
title | The title of book to search       
author | The author's last name is enough for filter the search      
publisher | The publisher of book to search 
category | The category of book to search 
title_split | How the given title is matched, options are:<ul><li>0 The exact title phrase must by matched</li><li>1 Default - All the words in title must be matched in whatever order</li><li>2 At least one word should match</li></ul>
book_id | Providing id means only one book should returned      
isbn | The ISBN or ISBN13 of book to search        
author_id | ID of the selected author    
publisher_id | ID of the selected publisher
category_id | ID of the selected category
after_year | Published this year or later   
before_year | Published this year or before   
results_type | In what form are the returned results, options are:<ul><li>metadata - (Default) Every book is extracted and an array of metadata is</li><li>ids - Only ids are returned</li></ul>

<aside class="notice">
Searching and extracting several books can be very slow at times, so instead of extracting every single book you may prefer only the ids of found books. In that case pass the option `results_type: 'ids'`.
</aside>

### Some example requests

It is recommended to use at least two parameters if you trying to get a specific book. Here some examples of search request

`GET http://bookshark.eu/api/v1/search?title=ανδρομαχη&author=ευριπιδης`

`GET http://bookshark.eu/api/v1/search?title=χομπιτ&author=τολκιν&results_type=metadata`

`GET http://bookshark.eu/api/v1/search?author=arthur%20doyle&after_year=2010`

`GET http://bookshark.eu/api/v1/search?isbn=978-960-14-1157-6`

`GET http://bookshark.eu/api/v1/search?title=αρχοντας&author=τολκιν&results_type=ids`

<aside class="warning">
Don't forget to url encode your requests 
ie. http://bookshark.eu/api/v1/search?title=σημεια και τερατα should become http://bookshark.eu/api/v1/search?title=σημεια%20και%20τερατα before making the request.
</aside>

# Other Metadata Types

## Author

```shell
# Extracts the author with that specific id.
curl "http://bookshark.eu/api/v1/author?id=10207"
curl "http://bookshark.eu/api/v1/author?url=http://www.biblionet.gr/author/10207/"
```

> The above command return JSON structured like this:

```json
{
  "author": [
    {
      "name": "Tolkien, John Ronald Reuel",
      "firstname": "John Ronald Reuel",
      "lastname": "Tolkien",
      "extra_info": "1892-1973",
      "image": "http://www.biblionet.gr/images/persons/10207.jpg",
      "bio": "Ο John Ronald Reuel Tolkien, άγγλος φιλόλογος και συγγραφέας, γεννήθηκε το 1892 στην πόλη Μπλουμφοντέιν...",
      "award": [
        {
          "name": "The Benson Medal [The Royal Society of Literature]",
          "year": "1966"
        }
      ],
      "b_id": "10207"
    }
  ]
}
```

This endpoint retrieves a specific author, based on its biblionet id or url.

### HTTP Request

`GET http://bookshark.eu/api/v1/author?parameters`

### Query Parameters

In each request only one of id or uri should be used to specify an author.

Parameter | Description
--------- | -----------
id | The author's id in biblionet site.
uri | The author's url in biblionet site.

### JSON Response

The response is in the format: `{"metadata-type": [{ metadata-hash }]}`  
Here are the metadata-hash keys for the author object:

* name
* firstname
* lastname
* extra_info
* bio
* image
* award (Array of awards)
  * name
  * year
* b_id

<aside class="warning">
Be careful — Most of the above values may be null. Always check if a value is present before use.
</aside>

## Publisher

```shell
# Extracts the publisher with that specific id.
curl "http://bookshark.eu/api/v1/publisher?id=20"
curl "http://bookshark.eu/api/v1/publisher?url=http://biblionet.gr/com/20/"
```

> The above command return JSON structured like this:

```json
{
  "publisher": [
    {
      "name": "Εκδόσεις Πατάκη",
      "owner": "Στέφανος Πατάκης",
      "bookstores": {
        "Κεντρική διάθεση": {
          "address": [
            "Εμμ. Μπενάκη 16",
            "106 78 Αθήνα"
          ],
          "telephone": [
            "210 3831078"
          ]
        },
        "Γενικό βιβλιοπωλείο Πατάκη": {
          "address": [
            "Ακαδημίας 65",
            "106 78 Αθήνα"
          ],
          "telephone": [
            "210 3811850",
            "210 3811740"
          ]
        },
        "Έδρα": {
          "address": [
            "Παναγή Τσαλδάρη 38 (πρ. Πειραιώς)",
            "104 37 Αθήνα"
          ],
          "telephone": [
            "210 3650000",
            "210 5205600"
          ],
          "fax": "210 3650069",
          "email": "info@patakis.gr",
          "website": "www.patakis.gr"         
        }
      },
      "b_id": "20"
    }
  ]
}
```

This endpoint retrieves a specific publisher, based on its biblionet id or url.

### HTTP Request

`GET http://bookshark.eu/api/v1/publisher?parameters`

### Query Parameters

In each request only one of id or uri should be used to specify a publisher.

Parameter | Description
--------- | -----------
id | The publisher's id in biblionet site.
uri | The publisher's url in biblionet site.

### JSON Response

The response is in the format: `{"metadata-type": [{ metadata-hash }]}`  
Here are the metadata-hash keys for the publisher object:

* name
* owner
* bookstores (A hash of bookstores in the form of bookstore_title => {bookstore-data}})
  * address (Array of addresses)
  * telephone (Array of telephones)
  * fax
  * email
  * website
* b_id 

## Category

```shell
# Extracts the category and its parents/children with that specific id.
curl "http://bookshark.eu/api/v1/category?id=1041"
curl "http://bookshark.eu/api/v1/category?url=http://biblionet.gr/index/1041/"
```

> The above command return JSON structured like this:

```json
{
  "category": [
    {
      "192": {
        "ddc": "500",
        "name": "Φυσικές και θετικές επιστήμες",
        "parent": null
      },
      "1040": {
        "ddc": "520",
        "name": "Αστρονομία",
        "parent": "192"
      },
      "1041": {
        "ddc": "523",
        "name": "Πλανήτες",
        "parent": "1040"
      },
      "780": {
        "ddc": "523.01",
        "name": "Αστροφυσική",
        "parent": "1041"
      },
      "2105": {
        "ddc": "523.083",
        "name": "Πλανήτες - Βιβλία για παιδιά",
        "parent": "1041"
      },
      "576": {
        "ddc": "523.1",
        "name": "Κοσμολογία",
        "parent": "1041"
      },
      "current": {
        "ddc": "523",
        "name": "Πλανήτες",
        "parent": "1040",
        "b_id": "1041"
      }
    }
  ]
}
```

> Each category's tree is extracted from root category to last child. An extra element with key=current is added to show which category was extracted.

This endpoint retrieves a specific category's hierarchy, based on its biblionet id or url. This gets you metadata about that category and also about its parents and children.

### HTTP Request

`GET http://bookshark.eu/api/v1/category?parameters`

### Query Parameters

In each request only one of id or uri should be used to specify a category.

Parameter | Description
--------- | -----------
id | The category's id in biblionet site.
uri | The category's url in biblionet site.

### JSON Response

The response is in the format: `{"metadata-type": [{metadata-hash}]}`  
Here are the metadata-hash keys for the category object:

Each category contains an array of category hierarchies in form: category-id => category-data 

* name
* ddc (Dewey Decimal Classification)
* b_id 
* parent

<aside class="notice">
Only root category will have a root parent. When extracting a category, its whole tree from root parent to last child will be added ordered by ddc. An extra element with key equal current is added to show which category was extracted.
</aside>


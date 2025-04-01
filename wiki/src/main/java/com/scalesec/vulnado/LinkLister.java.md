# Documentation: `LinkLister.java`

## Overview
The `LinkLister` class provides functionality to extract hyperlinks from a given URL. It uses the `Jsoup` library for HTML parsing and includes methods to retrieve links with additional validation for private IP addresses.

---

## Class: `LinkLister`

### Purpose
The `LinkLister` class is designed to fetch and process hyperlinks from a web page. It includes two methods:
1. `getLinks`: Retrieves all hyperlinks from a given URL.
2. `getLinksV2`: Adds validation to ensure the URL does not point to a private IP address before fetching hyperlinks.

---

## Methods

### 1. `getLinks(String url)`
#### Description
Fetches all hyperlinks (`<a>` tags) from the HTML content of the specified URL.

#### Parameters
| Name | Type   | Description                     |
|------|--------|---------------------------------|
| `url` | `String` | The URL of the web page to parse. |

#### Returns
| Type         | Description                          |
|--------------|--------------------------------------|
| `List<String>` | A list of absolute URLs extracted from the web page. |

#### Exceptions
| Type         | Description                          |
|--------------|--------------------------------------|
| `IOException` | Thrown if there is an issue connecting to the URL or fetching its content. |

#### Logic
1. Connects to the given URL using `Jsoup.connect(url).get()`.
2. Selects all `<a>` elements using `doc.select("a")`.
3. Extracts the absolute URL (`href` attribute) of each link and adds it to the result list.

---

### 2. `getLinksV2(String url)`
#### Description
Fetches hyperlinks from the specified URL, but first validates that the URL does not point to a private IP address.

#### Parameters
| Name | Type   | Description                     |
|------|--------|---------------------------------|
| `url` | `String` | The URL of the web page to parse. |

#### Returns
| Type         | Description                          |
|--------------|--------------------------------------|
| `List<String>` | A list of absolute URLs extracted from the web page. |

#### Exceptions
| Type         | Description                          |
|--------------|--------------------------------------|
| `BadRequest` | Thrown if the URL points to a private IP address or if any other error occurs during processing. |

#### Logic
1. Parses the URL using `new URL(url)` to extract the host.
2. Checks if the host starts with private IP address prefixes (`172.`, `192.168`, or `10.`).
   - If the host is private, throws a `BadRequest` exception with the message "Use of Private IP".
3. If the host is valid, calls the `getLinks` method to fetch hyperlinks.
4. Catches any exceptions and wraps them in a `BadRequest` exception.

---

## Insights

### 1. **Private IP Validation**
The `getLinksV2` method ensures that the URL does not point to a private IP address. This is a security measure to prevent accessing internal or restricted network resources.

### 2. **Dependency on Jsoup**
The class relies on the `Jsoup` library for HTML parsing, which simplifies the process of extracting elements from web pages.

### 3. **Custom Exception Handling**
The `getLinksV2` method uses a custom exception (`BadRequest`) to handle errors, providing a clear indication of issues such as invalid URLs or private IP addresses.

### 4. **Scalability**
The methods are designed to handle a single URL at a time. For large-scale applications, additional features like concurrency or batch processing may be required.

### 5. **Potential Enhancements**
- **Timeout Handling:** Add timeout settings for `Jsoup.connect()` to handle slow or unresponsive URLs.
- **URL Validation:** Include checks for malformed URLs before processing.
- **Logging:** Implement logging for better debugging and monitoring.

---

## Dependencies
| Library | Purpose                          |
|---------|----------------------------------|
| `Jsoup` | HTML parsing and element selection. |
| `java.net.URL` | URL parsing and host extraction. |

---

## Exception: `BadRequest`
### Description
A custom exception used to indicate issues with the provided URL, such as pointing to a private IP address or other errors during processing.



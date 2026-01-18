# curl_to_padded_string

A header-only C++ library that loads a URL into a `simdjson::padded_string` using libcurl.


## Dependencies

- C++23 compiler
- libcurl
- simdjson (fetched automatically via CMake)

## Building

```bash
cmake -B build
cmake --build build
```

## Usage

Include the header and link against curl and simdjson:

```cpp
#include "curl_to_padded_string.h"

int main() {
    auto result = curl_to_padded_string::load_url("https://httpbin.org/json");
    if (result) {
        auto& padded = result.value();
        // Now you can parse with simdjson
        // simdjson::ondemand::parser parser;
        // auto doc = parser.iterate(padded);
        // ...
    } else {
        std::cerr << "Error: " << result.error().what() << std::endl;
    }
    return 0;
}
```

## API

```cpp
namespace curl_to_padded_string {
    std::expected<simdjson::padded_string> load_url(const std::string& url);
}
```

Returns a `simdjson::padded_string` on success, or an error on failure.



### Functions provided

```cpp

/// @brief Get the actual payload size of a URL without downloading the full content.
/// 
/// This function performs a full request to determine the content size
/// by using a write callback that counts bytes without storing them.
/// 
/// @param url The URL string to check (must be a valid HTTP/HTTPS URL).
/// @return The size in bytes of the content at the URL, or 0 if the request fails
///         or curl initialization fails.
inline size_t get_actual_payload_size(const std::string& url);

/// @brief Load the content of a URL into a padded string optimized for JSON parsing.
/// 
/// Downloads the full content from the URL and stores it in a simdjson padded_string,
/// which provides the necessary padding for efficient SIMD JSON parsing.
/// 
/// @param url The URL string to load (must be a valid HTTP/HTTPS URL).
/// @return An expected containing:
///         - On success: A padded_string with the downloaded content
///         - On failure: An unexpected with a runtime_error describing the issue
inline std::expected<simdjson::padded_string, std::runtime_error> load_url(const std::string& url);
```
## License

Public Domain (Unlicense)

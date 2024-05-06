# Information

## Structure of JSON
```json
[
    {
        "quote": "The only people who never fail are those who never try.",
        "author": "Ilka Chase",
        "link": "https://en.wikipedia.org/wiki/Ilka_Chase"
    },
    {
        "quote": "Failure is just another way to learn how to do something right.",
        "author": "Marian Wright Edelman",
        "link": "https://en.wikipedia.org/wiki/Marian_Wright_Edelman"
    },
]
```

## How to implement
```php
function get_random_inspiration_quote()
{
    $path_to_json = "https://raw.githubusercontent.com/wera-as/inspirational-quotes-source/main/quotes-new.json";
    $fallback_html_output = "<span class='insp_quote_heart'>Made with ♥️ by <a href='https://wera.no/' target='_blank'>Wera AS</a></span>";

    $json_content = @file_get_contents($path_to_json);
    if ($json_content === FALSE) {
        return $fallback_html_output;
    }

    $quotes_array = json_decode($json_content, true);
    if (json_last_error() != JSON_ERROR_NONE || !is_array($quotes_array) || count($quotes_array) == 0) {
        return $fallback_html_output;
    }

    $random_quote = $quotes_array[array_rand($quotes_array)];
    $author_html = isset($random_quote['author']) ? $random_quote['author'] : "Unknown";
    if (!empty($random_quote['link'])) {
        $author_html = "<a href='" . esc_url($random_quote['link']) . "' target='_blank' class='insp_quote_author'>" . esc_html($author_html) . "</a>";
    }

    $quote_text = isset($random_quote['quote']) ? esc_html($random_quote['quote']) : "No inspirational quote found.";
    $html_output = "<div class='insp_quote_content'><span class='insp_quote_quote'>" . $quote_text . "</span><span class='insp_quote_author'>" . $author_html . "</span>" . $fallback_html_output . "</div>";


    return $html_output;
}
```

## Basic CSS
```css

:root {
    --text-color: #0B1114;
    --link-color: #0073aa;
    --link-hover-color: #00a0d2;
    --font-family: 'Space Grotesk', sans-serif;
}

.insp_quote_content {
    position: absolute;
    clear: both;
    min-height: 100px;
    padding: 10px 20px;
    background-color: #f9f9f9;
    border-top: 1px solid #eee;
    text-align: center;
    margin-top: -50px;
    box-shadow: 0 -1px 1px rgba(0, 0, 0, 0.1);
    font-size: 16px;
    line-height: 1.6;
    color: var(--text-color);
    width: 96.5%;
    font-family: var(--font-family);
}


.insp_quote_quote {
    font-weight: 400;
    font-style: italic;
    display: block;
    margin-bottom: 8px;
    position: relative;
}

.insp_quote_author,
.insp_quote_heart {
    font-weight: 600;
    display: block;
}

.insp_quote_footer a,
.insp_quote_content a {
    color: var(--link-color);
    text-decoration: none;
    transition: color 0.3s;
}

.insp_quote_footer a:hover,
.insp_quote_content a:hover {
    color: var(--link-hover-color);
}

```

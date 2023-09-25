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
    $fallback_html_output = "<span class='insp_quote_hart'>Made with ♥️ by <a href='https://wera.no/' target='_blank'>Wera AS</a></span>";

    $json_content = @file_get_contents($path_to_json);

    if ($json_content === FALSE) {
        return $fallback_html_output;
    }

    $array = json_decode($json_content, true);

    if (json_last_error() != JSON_ERROR_NONE) {
        return $fallback_html_output;
    }

    if (!is_array($array) || count($array) <= 0) {
        return $fallback_html_output;
    }

    $one_item = $array[array_rand($array)];

    if (isset($one_item['link']) && !empty($one_item['link'])) {
        $author_html = "<a href='" . $one_item['link'] . "' target='_blank' class='insp_quote_author'>" . $one_item['author'] . "</a>";
    } else {
        $author_html = $one_item['author'];
    }

    $html_output = "&ldquo;<span class='insp_quote_quote'>" . $one_item['quote'] . "</span>&rdquo;<br><span class='insp_quote_author'>" . $author_html . "</span><br><br>" . $fallback_html_output;

    return $html_output;
}

add_filter('admin_footer_text', 'get_random_inspiration_quote');
```

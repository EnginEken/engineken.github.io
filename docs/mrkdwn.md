Tüm burada yazanlar ve daha fazlası bu dökümanda kullanılan `material theme` sayfasında bulunabilir. [Check out their page.](https://squidfunk.github.io/mkdocs-material/reference/abbreviations/)

!!! note
    This is note.

!!! note "note with headline"
	This is note with headline. All of them can be with headline

??? note "Collapsible Note"
	This is collapsible note. All of them can be collapsible.

!!! info
	This is info.

!!! tip
	This is tip.

!!! check
    This is check.

!!! warning
	This is warning.

!!! danger
	This is danger.

- There is also abstract, summary, tldr, todo, hint, important, success, done, question, help, faq, caution, attention, failure, fail, missing, error, bug, example, quote, cite

***

- Go Code Block with Line Number
- Code bloğunda herhangi iki satırı highlight etmek istersek `hl_lines="2 3"`(sadece 2 ce 3. satırlar) ya da `hl_lines="2-5"`(2,3,4,5. satırlar)

``` go title="go_code_block.go" linenums="1" hl_lines="4 5"
import "fmt"

func main() {
	speed := 100
	current_time := 2.5
	//Correct way
	speed = int(float64(speed) * current_time)
	fmt.Println(speed)
}
```

- Python Code Block with Line Number 

``` python title="python_code_block.py" linenums="1"
from requests import HTTPBasicAuth
```

- Yaml

``` yaml
theme:
  features:
    - content.code.annotate # (1)
```

- Highlighting inline code blocks

The `#!python range()` `#!go main()`function is used to generate a sequence of numbers.

- Grouping code blocks

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```

- Grouping other content

=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci

- Normal Console Output

```console
$ pip3 install mkdocs
```

- Şekilli Console Output with Termy

<div class="termy">

```console
$ uvicorn main:app --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
<span style="color: green;">INFO</span>:     Started reloader process [28720]
<span style="color: green;">INFO</span>:     Started server process [28722]
<span style="color: green;">INFO</span>:     Waiting for application startup.
<span style="color: green;">INFO</span>:     Application startup complete.
```

</div>

- Tables  


| Method      | Description                          |
| ----------- | ------------------------------------ |
| `GET`       | :material-check:     Fetch resource  |
| `PUT`       | :material-check-all: Update resource |
| `DELETE`    | :material-close:     Delete resource |
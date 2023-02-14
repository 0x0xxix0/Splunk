## Master cluster side

`$SPLUNK_HOME/etc/master-apps/<index_name_app>/local/indexes.conf`

<img width="438" alt="Screenshot 2023-02-14 at 09 43 50" src="https://user-images.githubusercontent.com/119075926/218671397-7f62895d-e5a0-4f05-b2fd-e6843722384d.png">

```
[test]
repFactor=auto
homePath=/Splunk/indexes/test/db/
coldPath=/Splunk/indexes/test/colddb/
thawedPath=/Splunk/indexes/test/thaweddb/
```


<img width="347" alt="Screenshot 2023-02-14 at 09 44 11" src="https://user-images.githubusercontent.com/119075926/218671463-178991d9-5a62-4707-820f-c403c8bbbdf6.png">

Run Distribute Configuration Bundle

<img width="1909" alt="Screenshot 2023-02-14 at 10 16 36" src="https://user-images.githubusercontent.com/119075926/218677995-bee27518-f15b-4b0a-b699-63de0efe5c5c.png">


Result

<img width="524" alt="Screenshot 2023-02-14 at 10 16 53" src="https://user-images.githubusercontent.com/119075926/218678055-e48d43a6-13ce-42f4-846e-51d84831aed9.png">


<img width="1911" alt="Screenshot 2023-02-14 at 10 17 39" src="https://user-images.githubusercontent.com/119075926/218678215-d490681e-e3ff-440a-a047-00719bcff87d.png">


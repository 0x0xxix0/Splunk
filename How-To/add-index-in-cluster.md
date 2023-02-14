## Master cluster side

`$SPLUNK_HOME/etc/master-apps/<index_name_app>/local/indexes.conf`

<img width="438" alt="Screenshot 2023-02-14 at 09 43 50" src="https://user-images.githubusercontent.com/119075926/218671397-7f62895d-e5a0-4f05-b2fd-e6843722384d.png">

```
[test]
repFactor=auto
homePath=$SPLUNK_DB/<index_name>/db 
coldPath=$SPLUNK_DB/<index_name>/cloddb
thawedPath=$SPLUNK_DB/<index_name>/thaweddb 
```

Run Distribute Configuration Bundle

<img width="1909" alt="Screenshot 2023-02-14 at 10 16 36" src="https://user-images.githubusercontent.com/119075926/218677995-bee27518-f15b-4b0a-b699-63de0efe5c5c.png">


Result

<img width="524" alt="Screenshot 2023-02-14 at 10 16 53" src="https://user-images.githubusercontent.com/119075926/218678055-e48d43a6-13ce-42f4-846e-51d84831aed9.png">


<img width="1911" alt="Screenshot 2023-02-14 at 10 17 39" src="https://user-images.githubusercontent.com/119075926/218678215-d490681e-e3ff-440a-a047-00719bcff87d.png">

## Indexers

Апка яка   прилетіла з МС

<img width="408" alt="Screenshot 2023-02-14 at 11 08 25" src="https://user-images.githubusercontent.com/119075926/218689803-6feaa057-ba2e-4bf9-94bd-c44808f31b39.png">


Де зберігаються дані 
<img width="510" alt="Screenshot 2023-02-14 at 11 04 08" src="https://user-images.githubusercontent.com/119075926/218688785-9f255139-6a3e-4be4-a49b-293211b3d6ac.png">


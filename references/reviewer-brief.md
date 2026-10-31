# Brief for a reviewing subagent

Fill in the project description and the paths, then hand this over with the batch.

> Classify tests by genre, so that useless genres can be deleted. Project: `<one line>`. You read, you change nothing in the repository, you write only the requested CSV.
>
> For each test in the batch, find it in its file by name (line numbers may have moved), read it IN FULL, and open the code it checks when that changes your answer. Never judge from the name.
>
> Check the keep rules in `<path>/genres.md` first, then give the first delete genre that applies. Do not keep a test merely out of doubt: name the keep rule it meets, with a reason specific to that test.
>
> CSV columns: `genre,file,line,test,reason`. `line` is the current line of the test. A test you cannot find is `NOT_FOUND`. Every test in the batch must appear: check the count with a script.
>
> Final answer, very short: the count per genre, what you did not read in full, any recurring genre missing from the list, any test that looks like it validates a bug, and any real product defect you saw, with file:line.

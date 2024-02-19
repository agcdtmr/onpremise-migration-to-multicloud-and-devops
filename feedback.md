[Maxim](https://adplist.org/mentors/maxim-danilov): I have seen your repo. If our meeting doesn't happen, here are some suggestions on what could be improved to transform your repo into an attractive portfolio:

Please unzip files and create folders for every mission. In my opinion, zip files should never be stored in the repo.
You have differences in EN and PT versions only in texts, but not in code. In this case, the DRY rule is completely broken in your repo. Here, you can use the gettext and i18n library in Python.
By the way, in PT templates, your language code is still "EN".
Some endpoint functions are too big! They are not possible to test somehow. They should be split. The KISS rule can help you.
You write a good description about "How to start," but in the description of the wanted position, I see "Automation and scripting (Python, Bash)." Please wrap all in working scripts. You can use "make"; I prefer to use "fab" (Fabric v2.0). But it should be learned for this position! i mean - instead a long description what i should to do, it should be simple, something like "To start simply clone repository and write 'make run' ".
See you in the future, and good luck!

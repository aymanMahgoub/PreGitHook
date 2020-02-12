# pre-commit hook

> In this wiki we will use pre-commit hook to automate very important and poring staff.

Git says
> Git has a way to fire off custom scripts when certain important actions occur. 
>There are two groups of these hooks: client-side and server-side. 
>Client-side hooks are triggered by operations such as committing and merging, 
>while server-side hooks run on network operations such as receiving pushed commits. 
>You can use these hooks for all sorts of reasons.

## why Pre-commit hook?  

* **The importance of history**.

> I personally believe any additional or regular work 
> (even if it is standard and we have to follow for clean code purpose)
> should be automated, but even we can automate this step we shouldn't lose the info of who/why write this code.

## Installing tools

* First we need a tool to fix any violations for code style.
```
You can chose any tool you want.
I used to coding with php and symfony framwork so here we will use php-cs-fixer and will add symfony rules.
it should not be problem with you, every language have it's tools and rules.
```

**Starting with Installing php-cs-fixer**

* You can follow this repo https://github.com/FriendsOfPHP/PHP-CS-Fixer and install it throw composer.
* You can install it throw docker too.
* Write this script in to your php dockerfile

```
ENV PHP_CS_FIXER_VERSION=2.14.0

RUN curl -L https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v$PHP_CS_FIXER_VERSION/php-cs-fixer.phar > /usr/local/bin/php-cs-fixer \
   && chmod +x /usr/local/bin/php-cs-fixer \
   && rm -rf /var/cache/apk/ */var/tmp/* /tmp/* 

```

## Creating our git hook.

* Create .githooks directory in to your project.
* Create pre-commit file in it and add the following script in it.

```
echo
echo "Check for any Violations for code style"
echo
echo "Run php-cs-fixer on changed files"
echo

 git diff --name-only --diff-filter=MA HEAD~1 | grep -e "\.php\|\.sh\|\.twig\|\.js\|\.css\|\.md$" | xargs -L1 php-cs-fixer --rules=@Symfony,-@PSR1,-blank_line_before_statement -q fix

FILES="$(git diff --staged --name-only HEAD --diff-filter=A)"

for filename in $FILES;
do
 git add $filename

done
```
* Of course you can add any message you want, use any tools you want with any rules you want, even better you can use git hooks to run any script you want and automate any regular step like deployment for example enjoy ;) .

## **please note that** 

* Unfortunately php-cs-fixer will fix any violations in all added file not only commit code so it is highly recommend to use it with new projects only.  

## Contributing

* Please feel very free to provide any change throw Pr or reporting problem throw issues here in this repo.

## Authors

* **Ayman Mahgoub** - *Initial work* - [aymanMahgoub](https://github.com/aymanMahgoub)

* See also the list of [contributors](https://github.com/aymanMahgoub/PreGitHook/contributors) who participated in this project.

## License

* This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/aymanMahgoub/PreGitHook/blob/master/LICENSE.md) file for details

## Acknowledgments

* https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks.

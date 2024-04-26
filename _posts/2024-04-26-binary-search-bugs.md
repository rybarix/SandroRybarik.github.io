# Binary search for bugs

Tracking bugs 🐛 in big projects is tricky.
A couple of merges a day by multiple developers can introduce subtle bugs.

It doesn't work right now but you know it worked last week.


To quickly find out which commit introduces a bug you can use **git bisect**.

It's **binary search** for the commit you're looking for.

**How to use it**:

1. You start the git bisect session with `git bisect start`
1. Then you tell it that the current commit is bad `git bisect bad`
1. We need to know what commit/tag was working `git bisect good 92498b`
1. Git will checkout to other commits and ask you if your software works on this commit
1. Type either `git bisect good` or `git bisect bad`
1. After a while, you'll find the commit which introduced the issue.

{% include nlcard.html %}

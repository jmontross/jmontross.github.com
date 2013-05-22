http://octopress.org/docs/deploying/github/


the workflow mostly
bundle exec rake new_post
bundle exec rake generate && bundle exec rake deploy
git add .
git commit -m 'awesome stuff'
git push origin source
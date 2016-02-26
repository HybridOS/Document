# search hzw git log
repo forall -p -c git log --author="Chih-Wei Huang" --format="%ce %h %cd %n %s %n %b" -p  android-6.0.0_r1..HEAD >../android-x86-6.0-hzw.log.txt

# search git log after some time 2015-10-08
repo forall -p -c git log --after={2015-10-08}

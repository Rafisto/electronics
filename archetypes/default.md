+++
date = '{{ .Date }}'
title = '{{ replace .File.ContentBaseName "-" " " | title }}
[params]
  math = true
+++

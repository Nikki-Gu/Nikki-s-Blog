# Latex IEEE模版颜色Bug

同时使用`\documentclass[journal,twoside,web]{ieeecolor}`和`\usepackage[table,xcdraw]{xcolor}`时出现部分文字变为蓝色的问题。

搜索发现这应该是一个模版里面的bug，可以通过引入ieeecolor之后添加如下代码进行fix：

```
% Fix ieeecolor's \caption
\usepackage{etoolbox}
\makeatletter
\@ifundefined{color@begingroup}%
  {\let\color@begingroup\relax
   \let\color@endgroup\relax}{}%
\def\fix@ieeecolor@hbox#1{%
  \hbox{\color@begingroup#1\color@endgroup}}
\patchcmd\@makecaption{\hbox}{\fix@ieeecolor@hbox}{}{\FAILED}
\patchcmd\@makecaption{\hbox}{\fix@ieeecolor@hbox}{}{\FAILED}
%
```

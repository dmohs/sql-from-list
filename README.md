# sql-from-list

```clojure
(let [{:keys [sql params]} (from-list
                            [:newlines
                             [:str "begin" ";"]
                             [:newlines
                              "select name from"
                              [:str ["select * from accounts"] " a"]
                              [:str "where "
                               [:and [:str "foo = " [:$i 1]]
                                [[:or
                                  [:str "bar = " [:$i 2]]
                                  [:str "baz = ANY" [[:$i [5 6 7]]]]]]]]]
                             ";"
                             "end;"])]
  (println sql)
  (u/cljslog params))
```
output:
```
begin;
select name from
(select * from accounts) a
where foo = $1 and (bar = $2 or baz = ANY($3))
;
end;
(1 2 [5 6 7])
```

- query-properties:: [:icon :page :updated-at]
  #+BEGIN_QUERY
  {:title [:h1 "⏳ Ongoing Projects"]
    :query [:find (pull ?page [*])
        :where
          [?block :block/page ?page]
          [?page :block/name ?pagename]
          [?block :block/marker ?marker]
          [(contains? #{"DOING"} ?marker)]
          (page-ref ?block "project")
          (not [?page :block/name "templates"])
      ]
      :breadcrumb-show? false
      :collapsed? false}
  #+END_QUERY

- query-properties:: [:icon :page :updated-at]
  #+BEGIN_QUERY
  {
  :title [:h1 "👨‍💼People"]
  ;; ---- Get every block into variable ?block
  :query [:find (pull ?block [*])
      ;; ---- filter command
      :where
      ;; ---- get page name (lowercase) from the special page block into variable ?pagename
      [?block :block/name ?pagename]
      ;; ---- Select if block is a special page block and has a single property (arg1) with value arg2
      (page-property ?block :tags "people")
  ]
  }
  #+END_QUERY

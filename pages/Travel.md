tags:: project page
icon:: 📂

- ## Project Meta
  collapsed:: true
	- DOING [#B] #project [[Travel]]
- ## Actions Log
	- query-properties:: [:deadline :priority :block]
	  #+BEGIN_QUERY
	  {:title [:h4 "On ToDo List"]
	  :query [:find (pull ?b [*])
	       :in $ ?tag
	       :where
	       [?b :block/marker ?marker]
	       [(contains? #{"TODO"} ?marker)]
	       (page-ref ?b ?tag)
	       [?ref :block/name "project"]
	       (not [?b :block/refs ?ref])]
	  :inputs [:query-page]
	  :result-transform :add-task-attrs
	  :breadcrumb-show? true
	  }
	  #+END_QUERY
	- query-table:: false
	  query-properties:: [:page :block]
	  #+BEGIN_QUERY
	  {:title [:h4 "Ongoing Tasks"]
	  :query [:find (pull ?b [*])
	       :in $ ?tag
	       :where
	       [?b :block/marker ?marker]
	       [(contains? #{"DOING" "NOW" "LATER" "WAITING"} ?marker)]
	       (page-ref ?b ?tag)
	       [?ref :block/name "project"]
	       (not [?b :block/refs ?ref])]
	  :inputs [:query-page]
	  :result-transform :add-task-attrs
	  :breadcrumb-show? true
	  :group-by-page? false
	  :collapsed? false
	  }
	  #+END_QUERY
	- query-properties:: [:deadline :priority :block]
	  collapsed:: true
	  #+BEGIN_QUERY
	  {:title [:h4 "Completed Tasks"]
	  :query [:find (pull ?b [*])
	       :in $ ?tag
	       :where
	       [?b :block/marker ?marker]
	       [(contains? #{"DONE"} ?marker)]
	       (page-ref ?b ?tag)
	       [?ref :block/name "project"]
	       (not [?b :block/refs ?ref])]
	  :inputs [:query-page]
	  :result-transform :add-task-attrs
	  :breadcrumb-show? true
	  :table-view? false
	  :collapsed? true
	  }
	  #+END_QUERY
- ## Issues Log
	- query-sort-by:: status
	     query-table:: true
	     query-sort-desc:: true
	     query-properties:: [:block :owner :status]
	  #+BEGIN_QUERY
	  {:title [:h4 "Open Issues"]
	  :query [:find (pull ?b [*])
	       :in $ ?query-page
	       :where
	       [?p :block/name ?query-page]
	  	 [?tag2 :block/name "issues"]
	  	 [?b :block/refs ?tag2]
	  	 [?tag1 :block/name "closed"]
	  	 (not [?b :block/refs ?tag1])
	       [?b :block/refs ?p]
	       [?ref :block/name "project"]
	       (not [?b :block/refs ?ref])         
	       ]
	  :inputs [:query-page]
	  :breadcrumb-show? false
	  :table-view? true
	  :group-by-page? false
	  :collapsed? false
	  }
	  #+END_QUERY
	- query-properties:: [:block :owner :status]
	  collapsed:: true
	  #+BEGIN_QUERY
	  {:title [:h4 "Closed Issues"]
	  :query [:find (pull ?b [*])
	       :in $ ?query-page
	       :where
	       [?p :block/name ?query-page]
	  	 [?tag2 :block/name "issues"]
	  	 [?b :block/refs ?tag2]
	  	 [?tag1 :block/name "closed"]
	  	 [?b :block/refs ?tag1]
	       [?b :block/refs ?p]
	       [?ref :block/name "project"]
	       (not [?b :block/refs ?ref])         
	       ]
	  :inputs [:query-page]
	  :breadcrumb-show? false
	  :table-view? true
	  :group-by-page? false
	  :collapsed? true
	  }
	  #+END_QUERY
- #+BEGIN_QUERY
  {:title [:h2 "Project Notes"]
  :query [:find (pull ?b [*])
       :in $ ?query-page
       :where
       [?p :block/name ?query-page]
       [?b :block/refs ?p]
  	 [?tag2 :block/name "issues"]
  	 (not [?b :block/refs ?tag2])
       [?ref :block/name "project"]
       (not [?b :block/refs ?ref])        
       (not [?b :block/marker _])
       ]
  :inputs [:query-page]
  :result-transform (fn [result]
                   (sort-by (fn [b]
                              (get b :block/created-at "A")) result))
  :breadcrumb-show? false
  :group-by-page? true
  :collapsed? false
  }
  #+END_QUERY

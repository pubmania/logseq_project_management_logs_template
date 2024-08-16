tags:: people
icon:: 👨‍💼

- ## Tasks
	- query-table:: true
	   query-properties:: [:priority :deadline :block]
	  #+BEGIN_QUERY
	  {:title [:h3 "Owned"]
	  :query [:find (pull ?b [*])
	     :in $ ?tag
	     :where
	     [?b :block/marker ?marker]
	     [(contains? #{"TODO" "DOING" "NOW" "LATER" "WAITING"} ?marker)]
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
	- query-table:: true
	  collapsed:: true
	   query-properties:: [:marker :deadline :block]
	  #+BEGIN_QUERY
	  {:title [:h3 "Closed or Cancelled"]
	  :query [:find (pull ?b [*])
	     :in $ ?tag
	     :where
	     [?b :block/marker ?marker]
	     [(contains? #{"DONE" "CANCELLED" "CANCELED" } ?marker)]
	     (page-ref ?b ?tag)
	     [?ref :block/name "project"]
	     (not [?b :block/refs ?ref])]
	  :inputs [:query-page]
	  :result-transform :add-task-attrs
	  :breadcrumb-show? true
	  :group-by-page? false
	  :collapsed? true
	  }
	  #+END_QUERY
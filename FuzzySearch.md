
https://shazwazza.github.io/Examine/searching.html

FUZZY SEARCH
==================================

```
var toSearchForm = searchTerm.Split(' ');

                var searcher = ExamineManager.Instance.SearchProviderCollection["SiteQuickSearchSearcher"];

                var criteria = searcher.CreateSearchCriteria(IndexTypes.Content);
                var fieldsToSearch = new[]
                                         {
                                             "nodeName", "pageTitle", "menuText", "pageBody", "metaKeywords",
                                             "metaDescription"
                                         };

                IBooleanOperation filter = criteria.GroupedOr(fieldsToSearch, toSearchForm.First().Fuzzy());

                foreach (var term in toSearchForm.Skip(1))
                    filter = filter.Or().GroupedOr(fieldsToSearch, term.Fuzzy());

                if (filter != null)
                {
                    filter = filter.Not().Field("umbracoNaviHide", "1");
                    var compiled = filter.Compile();
                    var query = searcher.Search(compiled);

                    return new PageSearchResultBuilder().BuildSearchResults(from r in query
                                                                            orderby r.Score descending
                                                                            select new Node(r.Id));
                }

```

Search Reference :: https://skrift.io/issues/examine-in-umbraco-8/

# sloths

when pandas is too much hassle

Sloths package targets a scenario when you want a dict of dicts.
Or a dict of dicts of dicts of lists. 

In 'normal' python, after a couple of levels this turns into a mess very quickly.

**before**
```python
# this one is ugly
year2company2employee2diplomas = defaultdict(lambda: defaultdict(lambda : defaultdict(list)))

# this one is more or less ok
for year in years:
    for company in companies:
        for name, surname, diploma in get_diplomas_for_year_and_compan(year, company)
            year2company2employee2diplomas[year][company][name, surname].append(diploma)


# when we need to iterate the data, it is just terrible
for year, company2employee2diplomas in year2company2employee2diplomas.items():
    for company, employee2diplomas in company2employee2diplomas.items():
        for (name, surname), diplomas in employee2diplomas.items():
            for diploma in diplomas:
                finally_we_can_do_something(year, company, name, surname, diploma)

# that's specially terrible if e.g. we only needed a list of all achievemnts for a company.
```

Now, pandas does not help much with data until you completely collected it. Appending data to pandas on-the-go is quite a bad idea.

(and has other issues like auto-conversion of types, which you don't want to happen to the data without seeing the effect).

Sloth essentially works as a universal storage, where you can throw data to change its shape later.

**after**
```python

sloth = Sloth()
sloth[year][company].append_at((name, surname), diploma)

for company, diplomas in sloth.iterate('year:company:name surname:[diploma] -> company [diploma]'):
    print(f'{company} has in total {len(diplomas)}')

# and that's it, diplomas are grouped by company
```

Nested collections are created automatically, and a list is also created automatically (since we pointed at this by using `append_at`).
There is no need to think about this forward anymore.

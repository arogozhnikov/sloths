# sloths

when pandas is too much hassle

Sloths package targets a scenario when you want a dict of dicts.
Or a dict of dicts of dicts of lists. 

In 'normal' python, after a couple of levels this turns into a mess very quickly.
```python
# this one is ugly
year2company2employee2achievements = defaultdict(lambda: defaultdict(lambda : defaultdict(list)))

# this one is more or less ok
for year in years:
    for company in companies:
        for name, surname, achievement in get_achievements_for_year_and_compan(year, company)
            year2company2employee2achievements[year][company][name, surname].append(achievement)


# when we need to iterate the data, it is just terrible
for year, company2employee2achievements in year2company2employee2achievements.items():
    for company, employee2achievements in company2employee2achievements.items():
        for (name, surname), achievements in employee2achievements.items():
            for achievement in achievements:
                finally_we_can_do_something(year, company, name, surname, achievement)

# that's specially terrible if e.g. we only needed a list of all achievemnts for a company.
```

Now, pandas does not help much with data until you completely collected it. Appending data to pandas on-the-go is quite a bad idea.

(and has other issues like auto-conversion of types, which you don't want to happen to the data without seeing the effect).

Sloth essentially works as a universal storage, where you can throw data to change its shape later.


```python

sloth = Sloth()
# nested collections are created automatically,
# and a list is also created automatically.
# no need to think about this forward
sloth[year][company].append_at((name, surname), achievement)


for company, achivements in sloth.iterate('year:company:name surname:[achivement] -> company [achievement]'):
    print(f'{company} has in total {len(achievements)}')

# and that's it, achievements are grouped by company
```
# India Election Result Analysis SQL Project

## Project Overview
This project is about analyzing the India General Election 2024 results using SQL. The data is stored in PostgreSQL and includes information about candidates, parties, constituencies, votes, and alliances.

## Objectives

This project analyzes the India Election 2024 results using PostgreSQL and SQL. It answers key questions like:
Seats won by parties and alliances (NDA, I.N.D.I.A, Others)
Winning candidates and vote margins
EVM vs postal vote distribution
State-wise and party-wise results
Great for practicing SQL joins, filters, and aggregations on real-world data.

### Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to find total seat in parliament**:
   
```sql
select 
count(distinct parliament_constituency) as total_seats 
from constituencywise_results ;
```
2. **What are the total number of seats available for elections in each state**

  ```sql
   select
s.state as state_name,
count(distinct cr.parliament_constituency) as total_seats
from
constituencywise_results cr
inner join statewise_results sr on cr.parliament_constituency= sr.parliament_constiruency
inner join states s on sr.state_id =s.state_id
group by s.state;
  ```


3. **Total Seats Won by NDA Alliance**

```sql
SELECT 
    SUM(CASE 
            WHEN party IN (
                'Bharatiya Janata Party - BJP', 
                'Telugu Desam - TDP', 
				'Janata Dal  (United) - JD(U)',
                'Shiv Sena - SHS', 
                'AJSU Party - AJSUP', 
                'Apna Dal (Soneylal) - ADAL', 
                'Asom Gana Parishad - AGP',
                'Hindustani Awam Morcha (Secular) - HAMS', 
				'Janasena Party - JnP', 
				'Janata Dal  (Secular) - JD(S)',
                'Lok Janshakti Party(Ram Vilas) - LJPRV', 
                'Nationalist Congress Party - NCP',
                'Rashtriya Lok Dal - RLD', 
                'Sikkim Krantikari Morcha - SKM'
            ) THEN won
            ELSE 0 
        END) AS NDA_Total_Seats_Won
FROM 
    partywise_results;
```

4. **Seats Won by NDA Alliance Parties**
   ```sql
   select 
    party as Party_Name,
    won as Seats_Won
   from partywise_results
   where party in (
        'Bharatiya Janata Party - BJP', 
        'Telugu Desam - TDP', 
		'Janata Dal  (United) - JD(U)',
        'Shiv Sena - SHS', 
        'AJSU Party - AJSUP', 
        'Apna Dal (Soneylal) - ADAL', 
        'Asom Gana Parishad - AGP',
        'Hindustani Awam Morcha (Secular) - HAMS', 
        'Janasena Party - JnP', 
		'Janata Dal  (Secular) - JD(S)',
        'Lok Janshakti Party(Ram Vilas) - LJPRV', 
        'Nationalist Congress Party - NCP',
        'Rashtriya Lok Dal - RLD', 
        'Sikkim Krantikari Morcha - SKM'
    )
   order by Seats_Won desc;
```

5. **Total Seats Won by I.N.D.I.A. Alliance**


```sql

select sum(case
 when party in
(
'Indian National Congress - INC',
                'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
                'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK'
) then won
else 0
end) as INDIA_total_seats_won
from partywise_results;

```

6. **Seats Won by I.N.D.I.A. Alliance Parties**
   ```sql

   select party as INDIA_alliance_parties,
   won as total_seats_won
   from partywise_results
   where party in
   ('Indian National Congress - INC',
                'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
                'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK')
   order by total_seats_won desc;
```

7. **Add new column field in table partywise_results to get the Party Alliance as NDA, I.N.D.I.A and OTHER.**



``sql
alter table partywise_results
add column party_alliance varchar(100);

update partywise_results
set party_alliance= case
when party in
(
'Bharatiya Janata Party - BJP', 
        'Telugu Desam - TDP', 
		'Janata Dal  (United) - JD(U)',
        'Shiv Sena - SHS', 
        'AJSU Party - AJSUP', 
        'Apna Dal (Soneylal) - ADAL', 
        'Asom Gana Parishad - AGP',
        'Hindustani Awam Morcha (Secular) - HAMS', 
        'Janasena Party - JnP', 
		'Janata Dal  (Secular) - JD(S)',
        'Lok Janshakti Party(Ram Vilas) - LJPRV', 
        'Nationalist Congress Party - NCP',
        'Rashtriya Lok Dal - RLD', 
        'Sikkim Krantikari Morcha - SKM'
)
then 'NDA'
when party in
(
'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
                'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK'
)
then 'INDIA'
else 'other'
end;
```

8. **Which party alliance (NDA, I.N.D.I.A, or OTHER) won the most seats across all states?**

```sql
select party_alliance, sum(won) as seat_won
from partywise_results
group by party_alliance
order by seat_won desc
limit 1;
```

9. **Winning candidate's name, their party name, total votes, and the margin of victory for a specific state and constituency?**

```sql
select cr.winning_candidate,
pr.party as party_name,
cr.total_votes,
pr.party_alliance,
cr.margin as margin_of_victory,
s.state,cr.parliament_constituency
from constituencywise_results cr
join partywise_results pr on pr.party_id= cr.party_id
join statewise_results sr on sr.parliament_constiruency = cr.parliament_constituency
join states s on s.state_id=sr.state_id
where s.state = 'Haryana'
and sr.constituency = 'FARIDABAD';
```
10. **What is the distribution of EVM votes versus postal votes for candidates in a specific constituency?**

```sql
select distinct
cd.candidate, cd.evm_votes, cd.postal_votes,cd.total_votes,
cr.constituency_name
from constituencywise_details cd
join constituencywise_results cr on cd.costituency_id= cr.constituency_id
where cr.constituency_name = 'MATHURA'
order by cd.total_votes desc;
```

### Findings

- BJP-led NDA won the highest number of seats across all states.
- Uttar Pradesh and Maharashtra had the most competitive constituencies.
- In many seats, the vote margin between winner and runner-up was very small.
- EVM votes made up the majority of total votes, but postal votes played a key role in close races.
- Some parties performed strongly only in specific states (e.g., DMK in Tamil Nadu, TMC in West Bengal).
- The I.N.D.I.A alliance did better in southern and eastern states.

### Reports

This project includes various SQL reports such as:

- Total seats won by each party and alliance (NDA, I.N.D.I.A, OTHER)
- State-wise party performance
- Winning candidate details (name, party, votes, margin)
- Runner-up vs winner comparison
- EVM vs Postal vote breakdown per candidate
- Top 10 candidates with highest votes
- Seats won in each state by each alliance
These reports help understand election trends using real data and SQL queries.

### Conclusion

This project shows how SQL can be used to explore and analyze real-world election data. By writing simple but powerful queries, we learned about party performance, vote patterns, and state-wise results. It’s a good example of how data analysis helps in understanding large datasets like national election results.




<img src="https://cdn.trangcongnghe.com.vn/uploads/posts/2023-09/one-pice.jpg" width="100%"/>
select crb.txn_date,
                         round(sum(crb.amount_lcy) / 1e6) as c102 --tgkkh
                   from source.saoke_crb crb
                   left join source.saoke_thauchi tc
                     on crb.app_id = tc.account_id
                    and tc.txn_date = crb.txn_date
                   join SMY.sector_g02354 sec
                     on crb.cus_sector = sec.sector
                  where 1=1--crb.txn_date = vtod
                    and (substr(crb.sbv_code, 1, 4) in
                        ('4211','4214','4221','4224','4231','4238',
                        '4241','4251','4254','4261','4264')
                        or substr(crb.sbv_code, 1, 3) in ('427', '428'))
                    and (tc.account_id is null or tc.working_balance > 0)
                    and crb.mat_date is null
					group by crb.txn_date
					having sum(crb.amount_lcy) > 0

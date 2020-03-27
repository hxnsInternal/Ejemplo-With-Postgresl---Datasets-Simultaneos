-- Ejemplo de uso de diferentes datasets de forma simultanea en clausula 'With'

with dataset1 as (select 1 id_producto, 'Telefonia 4g' producto, 202002 periodo ,true activo       -- Definicion de tuplas de ejemplo
                  union all
                  select 2 id_producto,'Telefonia 3g' producto ,201908 periodo ,false activo
                  union all
                  select 3 id_producto ,'Telefonia 3g' producto ,202003 periodo,true activo
                  union all
                  select 4 id_producto ,'Telefonia 4g' producto ,202001 periodo ,false activo),      
     dataset2 as (select d.id_producto                 -- Obtener los registros unicos por producto ordenado el periodo mas reciente
                         ,d.producto
                         ,d.periodo
                         ,rank() over (partition by d.producto order by d.periodo desc) rk
                  from dataset1 d
                 )
select d1.*           -- Listar todos los registros del dataset1
from dataset1 d1
    where d1.id_producto in(select d2.id_producto          -- invocar el resultado contenido en el dataset2 como filtro en el dataset1
                            from dataset2 d2 
                                   where d2.rk = 1);
# data-baseFormarting
## @model DynamicTableResult
@{
    ViewData["Title"] = "Campanha";
    var dt = string.Empty;
    var year = string.Empty;
    var day = string.Empty;
    var month = string.Empty; 
    var date = string.Empty;

}

<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.24/css/jquery.dataTables.css">

<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.11.5/css/jquery.dataTables.min.css">
<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/searchbuilder/1.3.2/css/searchBuilder.dataTables.min.css">
<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/datetime/1.1.2/css/dataTables.dateTime.min.css">

<div class="content">
    <div class="container-fluid">

        <div class="titulo">
            <h3>Carteira - @ViewBag.nomeCampanha</h3>
        </div>

        <div class="tb-content">

            @if (Model.HasData)
            {
                <table class="table table-hover table-striped" id="tabela" data-order='[[6, "desc"]]'>
                    <thead>
                        <tr>
                            @foreach (var column in Model.Data.First().Keys)
                            {
                                <th data-sorted="DD/MM/YYYY" scope="col">@column</th>
                            }
                            <th scope="col">#</th>
                        </tr>
                    </thead>

                    <tbody>
                        @foreach (var row in Model.Data)
                        {
                            <tr>
                                @foreach (var column in row)
                                {
                                    if (column.Key?.Equals("CNPJ") ?? false)
                                    {
                                        <td scope="col">@column.Value.toStringCnpj()</td>
                                    }
                                    else if (column.Key?.Equals("Última Compra") ?? false)
                                    {
                                        if (!string.IsNullOrEmpty(column.Value.RemoveSpace()))
                                        {
                                             dt = column.Value;
                                            date = dt.Replace("/","");
                                            year = date.Substring(4, 4);
                                            day = date.Substring(0, 2);
                                            month = date.Substring(2, 2);
                                            date = year +"-"+ month + "-" + day;
                                            
                                            <td data-order='@date' scope="col">
                                                @*<span style="display:none">@date</span>*@
                                                @column.Value
                                            </td>
                                        }
                                        else
                                        {
                                            <td scope="col">01/02/2022</td>                                            
                                        }
                                    }
                                    else
                                    {
                                        <td scope="col">@(column.Value ?? string.Empty)</td>
                                    }
                                }
                                @if (row.ContainsKey("CNPJ"))
                                {
                                    <td><a href="@Url.Action("Index", "Cliente", new { cnpj = row["CNPJ"] })" class="btn btn-primary">Acessar</a></td>
                                }
                                else
                                {
                                    <td></td>
                                }
                            </tr>
                        }   

                    </tbody>
                </table>
            }

        </div>

    </div>
</div>

@section Scripts{
    <script type="text/javascript" charset="utf8" src="https://code.jquery.com/jquery-3.5.1.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.11.5/js/jquery.dataTables.min.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/searchbuilder/1.3.2/js/dataTables.searchBuilder.min.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/datetime/1.1.2/js/dataTables.dateTime.min.js"></script>

    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.10.24/js/jquery.dataTables.js"></script>

    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/searchbuilder/1.0.1/js/dataTables.searchBuilder.min.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.8.4/moment.min.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/plug-ins/1.11.5/sorting/datetime-moment.js"></script>

    <script type="text/javascript" charset="utf8" src="https://cdnjs.cloudflare.com/ajax/libs/luxon/1.25.0/luxon.min.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/plug-ins/1.10.24/sorting/datetime-luxon.js"></script>



    <script>
        $(document).ready(function() {
            $.fn.dataTable.luxon('dd/MM/yyyy');

            $('#tabela').dataTable({
                //columnDefs: [ { type: 'num', 'targets': 6 } ],
                 language: {
                    url: '//cdn.datatables.net/plug-ins/1.10.22/i18n/Portuguese-Brasil.json', 
                    searchBuilder: {
                        title: {
                            0: 'Filtro customizado',
                            _: 'Filtros (%d)'
                        },
                        logicAnd: 'Se',
                        logicOr: 'Ou',
                        clearAll: 'Limpar tudo',
                        add: '+',
                        data: 'Coluna',
                        value: 'Valor',
                        condition: 'Condiçâo',
                        conditions :{
                                number:{
					                equals: 'Igual',
					                not: 'Diferente',
					                lt: 'Menor que',
					                lte: 'Menor que, igual',
					                gte: 'Maior que, igual',
					                gt: 'Maior que',
                                    between: 'Entre',
					                notBetween: 'Diferente entre',
					                empty: 'Vazio',
					                notEmpty: 'Não vazio',
                                 }, 
                                string: {
                                    contains: 'Contém',
                                    empty: 'Vazio',
                                    endsWith: 'Termina com',
                                    equals: 'Igual',
                                    not: 'Diferente',
                                    notContains: 'Não contém',
                                    notEmpty: 'Não vazio',
                                    notEndsWith: 'Não termina com',
                                    notStartsWith: 'Não começa com',
                                    startsWith: 'Começa com',
                                  },
                                date: {
                                    after: 'Depois',
                                    before: 'Antes',
                                    between: 'Entre',
					                empty: 'Vazio',
					                equals: 'Igual',
                                    not: 'Não',
					                notBetween: 'Não entre',
					                notEmpty: 'Não vazio',
                                },
                            
                            }
                        }
                    },
                    dom: 'Qlfrtip',
                    searchBuilder: {
                        columns: [0,1,2,3,4,5,6,7,8]
                    }
            });

        });

    </script>
}

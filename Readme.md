Enexure.Sql.Dynamic.Providers.TransactSql
====
[![Build status](https://ci.appveyor.com/api/projects/status/eaep3n9f87g9a29u/branch/master?svg=true)](https://ci.appveyor.com/project/Daniel45729/enexure-sql-dynamic-providers-transactsql/branch/master)

> PM> Install-Package [Enexure.Sql.Dynamic.Providers.TransactSql](http://www.nuget.org/packages/Enexure.Sql.Dynamic.Providers.TransactSql/)

TSql Provider for [Enexure.Sql.Dynamic](https://github.com/enexure/Enexure.Sql.Dynamic) which will generate a database command representing the given query

#How to use 

The first step is to construct your [query](https://github.com/enexure/Enexure.Sql.Dynamic):

	var tableA = new Table("TableA").As("a");

	var query = Query
	    .From(tableA)
	    .Where(Expression.Not(Expression.Eq(tableA.Field("Id"), Expression.Const(1))))
	    .Select(tableA.Field("Name"));

Get the command using the provider:

	// Get the DbCommand
	var command = Provider.GetCommand(query);

Then you can execute the command as per standard ADO.Net. The parameters will automatically be populated. 

	using (var connection = new SqlConnection(connectionString)) {
		connection.Open();

		using (var transaction = connection.BeginTransaction()) {
			var command = Provider.GetCommand(query);

			command.Connection = connection;
			command.Transaction = transaction;

			using (var reader = command.ExecuteReader()) {
				while (reader.Read()) {
					//...
				}
			}

			transaction.Commit();
		}
		connection.Close();
	}
	
You might also want to check out [Enexure.Fire.Data](https://github.com/Lavinski/Enexure.Fire.Data) to simplify command usage.

	using (var session = new Session(connection)) {
		
		var command = Provider.GetCommand(query);
		var results = session
			.CreateCommand(command)
			.ExecuteQuery()
			.ToList<dynamic>();
			
		session.Commit();
	}

# bd.cs
ADO.NET Helper Class


```csharp
using System;
using System.Data;
using System.Data.SqlClient;

public class bd
{


    private static string ConnectionStr = "Data Source=.;Initial Catalog=BACKOFFICE;user id=sa;password=saa;";

    public static string getStrConnection()
    {
        return ConnectionStr;
    }

    public static object Executer_scalar(string Query)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(Query, sqlConn);

            sqlConn.Open();

            return SqlCom.ExecuteScalar();


        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static int Executer_NonQuery(string Query)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(Query, sqlConn);

            sqlConn.Open();

            try
            {
                return SqlCom.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                return -1;
            }


        }
        catch (Exception ex)
        {
            return -1;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataTable load_Query(string Query, string table_name = "Table")
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(Query, sqlConn);

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataTable dt = new DataTable(table_name);

            sqlda.Fill(dt);

            return dt;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static SqlDataAdapter load_table_adp(string table_name)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand("SELECT * FROM " + table_name, sqlConn);

            sqlConn.Open();

            return new SqlDataAdapter(SqlCom);


        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataTable load_table(string table_name)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand("SELECT * FROM " + table_name, sqlConn);

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataTable dt = new DataTable(table_name);

            sqlda.Fill(dt);

            return dt;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataTable load_table(string table_name, string order_by)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand("SELECT * FROM " + table_name + " ORDER BY " + order_by, sqlConn);

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataTable dt = new DataTable(table_name);

            sqlda.Fill(dt);

            return dt;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataSet load_tables(params string[] table_name)
    {
        SqlConnection sqlConn = null;
        try
        {
            DataSet ds = new DataSet();

            SqlCommand SqlCom = default(SqlCommand);

            SqlDataAdapter sqlda = default(SqlDataAdapter);

            DataTable dt = default(DataTable);

            sqlConn = new SqlConnection(ConnectionStr);



            sqlConn.Open();

            for (var i = 0; i <= table_name.Length - 1; i++)
            {
                SqlCom = new SqlCommand("SELECT * FROM " + table_name[i], sqlConn);
                sqlda = new SqlDataAdapter(SqlCom);
                dt = new DataTable(table_name[i]);
                sqlda.Fill(dt);
                ds.Tables.Add(dt);
            }

            return ds;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataSet load_DataSet(string Query, string table_name = "Table")
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(Query, sqlConn);

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataSet ds = new DataSet();

            sqlda.Fill(ds, table_name);

            return ds;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataSet load_DataSet_S(params string[] Query)
    {
        SqlConnection sqlConn = null;
        try
        {
            DataSet ds = new DataSet();

            SqlCommand SqlCom = default(SqlCommand);

            SqlDataAdapter sqlda = default(SqlDataAdapter);

            DataTable dt = default(DataTable);

            sqlConn = new SqlConnection(ConnectionStr);



            sqlConn.Open();

            for (var i = 0; i <= Query.Length - 1; i++)
            {
                SqlCom = new SqlCommand(Query[i], sqlConn);
                sqlda = new SqlDataAdapter(SqlCom);
                dt = new DataTable("Table_" + i);
                sqlda.Fill(dt);
                ds.Tables.Add(dt);
            }

            return ds;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataTable load_ps(string ps_name, string[] params_name = null, object[] params_value = null)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(ps_name, sqlConn);

            SqlCom.CommandType = CommandType.StoredProcedure;

            if ((params_name != null) & (params_value != null))
            {
                for (var i = 0; i <= params_name.Length - 1; i++)
                {
                    SqlCom.Parameters.AddWithValue(params_name[i], params_value[i]);
                }
            }

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataTable dt = new DataTable(ps_name);

            sqlda.Fill(dt);

            return dt;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static DataSet load_ps_ds(string ps_name, string[] params_name = null, object[] params_value = null)
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(ps_name, sqlConn);

            SqlCom.CommandType = CommandType.StoredProcedure;

            if ((params_name != null) & (params_value != null))
            {
                for (var i = 0; i <= params_name.Length - 1; i++)
                {
                    SqlCom.Parameters.AddWithValue(params_name[i], params_value[i]);
                }
            }

            sqlConn.Open();

            SqlDataAdapter sqlda = new SqlDataAdapter(SqlCom);

            DataSet ds = new DataSet();

            sqlda.Fill(ds);

            return ds;

        }
        catch (Exception ex)
        {
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static SqlDataReader load_Reader(string Query, string table_name = "Table")
    {
        SqlConnection sqlConn = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            SqlCommand SqlCom = new SqlCommand(Query, sqlConn);

            sqlConn.Open();

            return SqlCom.ExecuteReader();

        }
        catch (Exception ex)
        {
            return null;
        }
    }

    public static object Execute_Transact(params string[] Query)
    {
        SqlConnection sqlConn = null;
        SqlTransaction tran = null;
        try
        {
            sqlConn = new SqlConnection(ConnectionStr);

            sqlConn.Open();

            tran = sqlConn.BeginTransaction();

            SqlCommand[] SqlCom = new SqlCommand[Query.Length + 1];

            for (var i = 0; i <= Query.Length - 1; i++)
            {
                SqlCom[i] = new SqlCommand(Query[i], sqlConn);
                SqlCom[i].Transaction = tran;
            }

            for (var i = 0; i <= Query.Length - 1; i++)
            {
                SqlCom[i].ExecuteNonQuery();
            }

            tran.Commit();

            return 0;

        }
        catch (Exception ex)
        {
            try
            {
                tran.Rollback();
            }
            catch (Exception exx)
            {
            }
            throw ex;
        }
        finally
        {
            try
            {
                sqlConn.Close();
            }
            catch (Exception ex)
            {
            }
        }
    }

    public static bool isConnect()
    {

        try
        {
            SqlConnection c = new SqlConnection(ConnectionStr);

            c.Open();

            c.Close();

            return true;

        }
        catch (Exception ex)
        {
            return false;
        }
    }


}



```

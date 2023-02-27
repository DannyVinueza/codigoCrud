# codigoCrud

  Boton buscar
  
          buscar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Connection con;
                try{
                    String idp;
                    idp = id.getText();
                    System.out.println(id);
                    System.out.println("\n");
                    con = getConection();//Metodo para la conexion a la BD
                    String query = "select * from productos;";// WHERE Id = " + cedulaIn + ";";
                    Statement s = con.createStatement();
                    ResultSet rs = s.executeQuery(query);
                    System.out.println(rs);

                    while(rs.next()){
                        if(idp.equals(rs.getString(1))){
                            clasificacion.getSelectedItem();
                            nombre.setText(rs.getString(3));
                            cantidad.setText(rs.getString(4));
                            precio.setText(rs.getString(5));
                            seccion.getSelectedItem();

                            System.out.println(rs.getString(1) + " " +
                                    rs.getString(2) + " " +
                                    rs.getString(3) + " " +
                                    rs.getString(4) + " " +
                                    rs.getString(5) + " " +
                                    rs.getString(6));
                            /*nombre.setEnabled(true);
                            celular.setEnabled(true);
                            email.setEnabled(true);
                            actualizarB.setEnabled(true);*/
                            //break;
                        }else{
                            salida.setText("Producto no encontrado");
                            break;
                        }
                    }
                    con.close();
                }catch(HeadlessException | SQLException f){
                    System.err.println(f);
                }
            }
        });
        
Boton Crear
    
    crear.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Connection con;
                try{

                    con = getConection();
                    ps = con.prepareStatement("INSERT INTO registro (idproductos, clasificacion, nombre, cantidad, precio, seccion) VALUES (?, ?, ?, ?, ?, ?);");
                    ps.setString(1, id.getText());
                    ps.setString(2, (String)clasificacion.getSelectedItem());
                    ps.setString(3, nombre.getText());
                    ps.setString(4, cantidad.getText());
                    ps.setString(5,precio.getText());
                    ps.setString(6, (String)seccion.getSelectedItem());
                    System.out.println(ps);//Imprime para ver los datos ingresados por consola

                    int res = ps.executeUpdate();
                    if(res > 0){
                        JOptionPane.showMessageDialog(null, "Registro Guardado satisfactoriament");
                    }else{
                        JOptionPane.showMessageDialog(null,"Error al guardar el registro");
                    }
                    con.close();
                }catch(HeadlessException | SQLException f){
                    System.err.println(f);
                }
            }
        });
Boton Actualizar
             
            actualizar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Connection con;

                try{
                    con = getConection();
                    ps = con.prepareStatement("UPDATE productos SET idproductos = ?, clasificacion = ?, nombre = ?, cantidad = ?, precio = ?, seccion = ? WHERE id ="+ id.getText() );
                    //ps.setString(1, id.getText());
                    ps.setString(2, (String)(clasificacion.getSelectedItem()));
                    ps.setString(3, nombre.getText());
                    ps.setString(4, cantidad.getText());
                    ps.setString(5, precio.getText());
                    ps.setString(6, (String)(seccion.getSelectedItem()));
                    System.out.println(ps);//Imprime para ver los datos ingresados por consola
                    int res = ps.executeUpdate();
                    if(res > 0){
                        JOptionPane.showMessageDialog(null, "Actualizacion exitosa ");
                    }else{
                        JOptionPane.showMessageDialog(null,"Error al actualizar");
                    }
                    con.close();
                }catch(HeadlessException | SQLException f){
                    System.err.println(f);
                }
            }
        });
  
  Boton Eliminar

            eliminarButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Connection con;
                try{
                    con = getConection();
                    String conElim = "SELECT COUNT(*) FROM stock WHERE idstock = ?";
                    PreparedStatement existeD = con.prepareStatement(conElim);
                    existeD.setString(1, id.getText());
                    ResultSet rsD = existeD.executeQuery();
                    rsD.next();
                    int counD = rsD.getInt(1);

                    if(counD > 0){
                        ps = con.prepareStatement("DELETE FROM stock WHERE idstock = " + id.getText());
                        int cont =ps.executeUpdate();

                        if(cont > 0){
                            JOptionPane.showMessageDialog(null, "Producto Eliminado");
                        }else{
                            JOptionPane.showMessageDialog(null, "Error al eliminar el producto");
                        }
                    }else{
                        JOptionPane.showMessageDialog(null,"No se ecunetra el id para eliminar");
                    }
                }catch (SQLException s){
                    System.out.println(s);
                }
            }
        });

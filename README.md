import React from 'react';

const ServiceList = ({ services, user, onDelete, onEdit, onOrder, t }) => {
  if (!services.length) {
    return <p>{t.noServices}</p>;
  }

  return (
    <div className="service-list">
      <h3>{t.services}</h3>
      <ul>
        {services.map((service, idx) => (
          <li key={idx} style={{border: '1px solid #ddd', margin: '10px', padding: '10px'}}>
            <h4>{service.name}</h4>
            <p>{service.description}</p>
            <p>{t.price || 'السعر'}: {service.price}</p>
            {user?.role === 'admin' && (
              <>
                <button onClick={() => onEdit(idx)}>{t.editService}</button>
                <button onClick={() => onDelete(idx)} style={{marginLeft: 8, color: 'red'}}>{t.delete}</button>
              </>
            )}
            {user?.role === 'client' && (
              <button onClick={() => onOrder(service)}>{t.order}</button>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default ServiceList;
